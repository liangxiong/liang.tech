# Shutdown Hook
## 作用
- 应用正常退出（所有线程完成时，或在调用 System.exit(0) 时），执行特定的业务逻辑或关闭资源等操作
- 应用非正常退出（用户按下 Ctrl+C、操作系统关闭(kill pid，不带-9)），在退出时执行必要的挽救措施

## 用法：
```
Runtime.getRuntime().addShutdownHook(new Thread(() -> System.out.println("Shutdown Hook is running !")));
```
## 注意点

- 在某些情况下，可能无法执行 Shutdown Hook
  - JVM 由于某些内部错误而崩溃,则它可能崩溃而没有机会执行一条指令
  - Runtime.halt() 方法来终止 JVM，而不允许运行 Shutdown Hook
  - 操作系统发出 SIGKILL 信号（在 Unix/Linux 中为 kill -9）或 TerminateProcess（Windows）,则要求应用程序立即终止而无需甚至在等待任何清理活动

- 启动后，可以在完成之前强行关闭 Shutdown Hook
  - 在操作系统关闭的情况下，有可能在 Shutdown Hook 完成之前将其终止
    - 给出 SIGTERM，OS 将等待进程终止指定的时间，如果该过程未在此期限内终止，则操作系统将通过发出SIGTERM（或Windows中的对应程序）来强制终止该过程

  - 所以：
    - 快速完成，不会引起死锁
    - 不应该 执行长时间计算或等待用户 I/O 操作

- 可以有多个 Shutdown Hook，但是不能保证它们的执行顺序

- 关闭程序开始后，无法注册/取消注册 Shutdown Hook
  - 否则抛出 IllegalStateException 异常

- 关闭程序开始后，Shutdown Hook 开始执行,只能由 Runtime.halt() 停止

- 使用 Shutdown Hook 需要安全权限
  - 需要具有 shutdownHooks 权限
  - 如果在安全的环境中未经许可调用此方法，则将导致 SecurityException
```
SecurityManager sm = System.getSecurityManager();
if (sm != null) {
  sm.checkPermission(new RuntimePermission("shutdownHooks"));
}
```
