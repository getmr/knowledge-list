# 1. 事件循环得创建、获取、设置
- （1）asyncio.get_running_loop() python3.7新添加的

- （2）asyncio.get_event_loop()  # 如果loop已存在，等同于asyncio.get_running_loop()

- （3）asyncio.set_event_loop(loop)

- （4）asyncio.new_event_loop()

# 2. 运行和停止事件循环
- （1）loop.run_until_complete(future) 运行事件循环，直到future运行结束

- （2）loop.run_forever() 在python3.7中已经取消了，表示事件循环会一直运行，直到遇到stop。

- （3）loop.stop() 停止事件循环

- （4）loop.is_running() 如果事件循环依然在运行，则返回True

- （5）loop.is_closed() 如果事件循环已经close，则返回True

- （6）loop.close() 关闭事件循环
# 3. 创建Future和Task
- （1）loop.create_future(coroutine) ，返回future对象

- （2）loop.create_task(corootine)，返回task对象

- （3）loop.set_task_factory(factory)

- （4）loop.get_task_factory()

-  (5) asyncio.ensure_future(obj, *, loop=None)

# 4. 安排协程
asyncio.gather(*aws, loop=None, return_exceprions=False)
asyncio.wait(aws, *, loop=None, timeout=None, return_whenALL_COMPLETED)
aysncio.wait_for(fut, timeout, *, loop=None)
