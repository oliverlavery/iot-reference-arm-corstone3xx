Notes.md

Nano newlib is here:

/Applications/ArmGNUToolchain/13.2.Rel1/arm-none-eabi/arm-none-eabi/lib/thumb/v7-m/nofp

Currently homebrew (Arm) toolchain doesn't have libssp compiled in:

arm-none-eabi-gcc -ffreestanding -mthumb -mcpu=cortex-m3 -fstack-protector -Wall -Wextra -Wshadow -g3 -Os -ffunction-sections -fdata-sections -MMD -MP -MF"output/RTOSDemo.out" -MT output/RTOSDemo.out -I./../../../../Source/include -I./../../../../Source/portable/GCC/ARM_CM3 -I./../../../../Demo/Common/include -I./../../../../Demo/CORTEX_MPS2_QEMU_IAR_GCC -I./../../../../Demo/CORTEX_MPS2_QEMU_IAR_GCC/CMSIS -T ./mps2_m3.ld -Xlinker -Map=./output/RTOSDemo.map -Xlinker --gc-sections -nostartfiles -specs=nano.specs -specs=nosys.specs  ./output/tasks.o ./output/list.o ./output/queue.o ./output/timers.o ./output/event_groups.o ./output/stream_buffer.o ./output/heap_4.o ./output/port.o ./output/AbortDelay.o ./output/BlockQ.o ./output/blocktim.o ./output/countsem.o ./output/death.o ./output/dynamic.o ./output/EventGroupsDemo.o ./output/GenQTest.o ./output/integer.o ./output/IntQueue.o ./output/IntQueueTimer.o ./output/IntSemTest.o ./output/MessageBufferAMP.o ./output/MessageBufferDemo.o ./output/PollQ.o ./output/QPeek.o ./output/QueueOverwrite.o ./output/QueueSet.o ./output/QueueSetPolling.o ./output/recmutex.o ./output/semtest.o ./output/StaticAllocation.o ./output/StreamBufferDemo.o ./output/StreamBufferInterrupt.o ./output/TaskNotify.o ./output/TaskNotifyArray.o ./output/TimerDemo.o ./output/main.o ./output/main_blinky.o ./output/main_full.o ./output/startup_gcc.o ./output/RegTest.o ./output/printf-stdarg.o -o ./output/RTOSDemo.out
/Applications/ArmGNUToolchain/13.3.rel1/arm-none-eabi/bin/../lib/gcc/arm-none-eabi/13.3.1/../../../../arm-none-eabi/bin/ld: cannot find -lssp_nonshared: No such file or directory
/Applications/ArmGNUToolchain/13.3.rel1/arm-none-eabi/bin/../lib/gcc/arm-none-eabi/13.3.1/../../../../arm-none-eabi/bin/ld: cannot find -lssp: No such file or directory
collect2: error: ld returned 1 exit status
make: *** [output/RTOSDemo.out] Error 1


--- ARM Toolchain downloads from their website do not enable libssp. Instead we switch to unbuntu arm-none-eabi-gcc and it works!


Switching to iot-reference-arm-corstone3xx

1. Follow instructions here:
https://github.com/FreeRTOS/iot-reference-arm-corstone3xx/blob/main/docs/development_environment/linux_dev_env.md

2. Install the Corstone-315 model from here: https://developer.arm.com/downloads/-/arm-ecosystem-fvps

3. Add FVP model to path. For instance on linux:
```
PATH=$PATH:~/FVP_Corstone_SSE-315/models/Linux64_GCC-9.3/
```

Setup for freertos heap code:

Set this in components/freertos_kernel/CMakeLists.txt
set(FREERTOS_HEAP "${CMAKE_CURRENT_LIST_DIR}/library/portable/MemMang/heap_4.c" CACHE STRING "" FORCE)
