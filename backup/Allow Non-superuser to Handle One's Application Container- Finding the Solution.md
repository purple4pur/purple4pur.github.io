### Background

A friend of mine is hosting a QQ bot ( [Miao-Yunzai](https://github.com/yoimiya-kokomi/Miao-Yunzai) ) on my VPS, and let's call him friendR here. Recently I'm switching the bot to my new VPS, and trying to host as much as possible services in docker in order to keep the host machine clean.

After some experiments I can use my modified Dockerfile and compose file to invoke the bot in container as I want. The next challenge is to allow friendR to up/down the bot container and run `npm run app` in it, because he might restart the bot anytime he want and does not need my involvement.

So my goals are:

- **Security**: friendR should never be a superuser or in the docker group, or have any change to run arbitrary codes.
- **Independence**: friendR should independently up/down a specific container, or run specific commands in the container.
- **Volume run-through**: friendR should be able to modify bot related files bound into the container, easily.

### Try#1: shell script w/ SUID

With SUID bit one can run the executable as the owner. So my basic thinking is to place a simple shell script like `docker compose -f /path/to/compose.yaml up -d` into friendR's path, set its owner to superuser, and set file mode to `5750 (rwsr-x--T)`.

But here are two problems. The greatest one is SUID bit will be ignored on shebang script, which means friendR still has no permission to call docker commands. The other one is also very critical to security, that is friendR can overwrite docker related files (Dockerfile, compose.yaml, etc.) because they are in his controlled directory.

### Try#2: finite sudo permission

I want to solve the permission issue first. I set the root owned `up.sh` to `0700 (rwx------)`, and allow friendR to sudo on this specific file (via `visudo`), so that he can call with `sudo ./up.sh`:

```diff
+friendR ALL=(root) /path/to/up.sh
```

Yeah it works, but the security issue is even more obvious: friendR can simply overwrite `up.sh` to run anything as superuser. That is crazy and is definitely not acceptable.

A possible fix is to prepend the file hash to the file path in `sudoers`, while I think it's way too inflexible.

### Try#3: binary command wrapper w/ SUID (This works!)

During the research of Try#1, I noticed a [Stack Overflow answer](https://unix.stackexchange.com/a/369) saying that one may write a script wrapper to call the shell to grant other user the superuser permission.

I tried the c programming and quickly found the first issue: `system()` is to start a new process to run the following command, but not using the euid (Effective UID, influenced by SUID). That means friendR will not have permissions to call docker commands when he invokes the compiled binary.

I didn't dive deep into how `system()` works with UID, because later I found using `exec()` can get rid of this issue. `exec()` is to replace current process with its argument command, which inherits process' euid.

In this way, I just copy the binaries to friendR's directory and set mode to `4750 (rwsr-x---)`, then friendR can call them to invoke hard-coded docker commands.

### Bonus: complex commands wrapper workaround

One thing I didn't mention in Try#3 is actually the euid mechanism failed when I called `./up.sh` or `bash ./up.sh` with `exec()`. Somehow it might be also a security guard.

An enhancement to invoke `npm run app` in the container I want to achieve is to check if there's already a running process before starting the app. I wrote this check using shell script and finally realized I could hardly run the whole script in a single `exec()` calling.

Before my giving up suddenly it hit me that it's way easier to do this check inside the container, instead of using `docker compose top` outside. So I just leave a script calling to `docker exec`, and finish the rest of jobs inside the container.

```c
// run_app.c
execlp("docker", "docker", "compose", "-f", "/path/to/compose.yaml", "exec", "-it", "miao-yunzai", "./run_app.sh", (char *)NULL);
```

```bash
# run_app.sh
if [[ "$(ps -aux | grep 'npm run app' | grep -v grep)" != "" ]]; then
    echo "app is already running! will not start a 2nd one."
    exit 1
fi
echo "> npm run app"
npm run app
```

### Final solution

To keep it secured, all the docker related files are placed in my directory. Bot needed files are placed in friendR's directory, and bound into the container.

C docker wrappers and simple Makefile:

```c
// up.c as an example
#include <unistd.h>

int main() {
    execlp("docker", "docker", "compose", "-f", "/path/to/compose.yaml", "up", "-d", (char *)NULL);
    return 0;
}
```

```makefile
# Makefile
# usage: make -B

all: $(patsubst %.c,%,$(wildcard *.c))

%: %.c
	gcc $< -o $@
	sudo cp $@ /home/friendR/Miao-Yunzai
	sudo chown root:friendR /home/friendR/Miao-Yunzai/$@
	sudo chmod 4750 /home/friendR/Miao-Yunzai/$@
```

### Conclusion

After a few days of work finally it's solved. Providing a command wrapper in binary seems the only safe way to support this kinda crazy goal: letting non-superuser to manipulate docker while keep any other thing non-privileged.

It's hard to say this is a elegant solution but it works and, not too ugly I think? This can also be a useful reference whenever there's a similar scenario.