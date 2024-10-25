## vcs

```bash
-full64 +v2k -sverilog              # 固定起手（v2k 指 Verilog-2001）
-lca -debug_access+all -kdb         # 保存波形需要，debug 开关极大影响仿真时间[1][2]
-timescale=1ns/100ps                # 时间精度，考虑在这里统一指定
-ntb_opts uvm-1.1                   # UVM
+incdir+<XX_DIR> <XX_file>          # rtl、verif 文件
+define+<XX> +define+<XX>=<YY>      # 设置 define
+vcs+fsdbon                         # 自动生成波形文件（novas.fsdb）
-cm line+branch -cm_dir <cov.vdb>   # 统计覆盖率[3][4]
-fgp                                # 启用多线程仿真支持[5]
-j<NUM>                             # 多线程编译，实测对编译速度可能几乎无提升
-R [simv_options]                   # 编译后自动开始仿真
```

## simv

```bash
+UVM_TESTNAME=<TEST>                # 指定 run_test() 调用的 uvm_test
+ntb_random_seed=<SEED>             # 随机数种子，用于 random()、randomize() 等
-ucli -i <UCLI_FILE>                # 灵活控制是否生成波形
+fsdb+delta                         # 记录 delta time，能在 verdi 里展开
-cm line+branch -cm_dir <cov.vdb> -cm_name <TEST>
                                    # 统计覆盖率[3][4]
+UVM_TR_RECORD +UVM_LOG_RECORD +UVM_VERDI_TRACE
                                    # UVM debug 开关，能在 verdi 里可视化 transaction
+UVM_VERBOSITY=<VERBOSITY>          # 灵活调整 UVM 打印冗余级别
-fgp=num_threads:<NUM>,num_fsdb_threads:<NUM>,allow_less_cores -Xdprof=timeline
                                    # 多线程仿真[5]，实测对仿真速度可能几乎无提升
```

### <UCLI_FILE>

```bash
fsdbDumpvars 0                      # 生成所有波形，默认文件名为 novas.fsdb，极大影响仿真时间[1]
run
quit
```

## verdi

```bash
-dbdir simv.daidir                  # 加载代码
-ssf novas.fsdb                     # 加载波形，如果存在关联的代码也会自动加载（即省略 -dbdir）
-cov -covdir <cov.vdb>              # 打开 vdCoverage 窗口，并加载覆盖率信息
```

* `<Shift+滚轮>`：前后平移波形
* `双击波形边沿`：在代码窗口中跳转到对应的激励
* `nWave->Tools->Transaction Debug->Transaction and Protocol Analyzer`：打开可视化 transaction 面板
* `tProtocolAnalyzer->View->Sync. With nWave`：关联上下两个面板（波形、transaction）
* `Tools->Coverage`：打开 vdCoverage 窗口
* `vdCoverage->Tools->Generate URG Report`：生成覆盖率报告

## Reference

[1] [vcs中debug选项、波形dump对仿真时间的影响_kevindas的博客-CSDN博客](https://blog.csdn.net/kevindas/article/details/107307654)
[2] [Synopsys VCS 编译时，启用debug选项_XtremeDV的博客-CSDN博客](https://blog.csdn.net/zhajio/article/details/88839838)
[3] [vcs覆盖率选项_weixin_39662684的博客-CSDN博客](https://blog.csdn.net/weixin_39662684/article/details/108255556)
[4] [VCS覆盖率使用详解（基础、合并、查看、分析） - 知乎](https://zhuanlan.zhihu.com/p/620471082)
[5] [VCS -- fgp 仿真加速 - Thisway2014 - 博客园](https://www.cnblogs.com/thisway2014/p/16783601.html)