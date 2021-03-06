# GumOS
**WORK IN PROGRESS**<br>
Not a real operating system, but a wrapper on FreeRTOS and M5Stack. Will support lua script for M5Stack(currently lua does not support drawing and working with hardware – writing library implementing all of those is still to be done)

# Notice 
GUI is not yet ready. Also I have no idea why git don't want to add ramfs folder completely and adds only logo.

# Modules
As I don't know how to implement dynamic loading on esp32 modules are currently hardcoded in <a href="https://github.com/Andrewerr/GumOS/blob/master/src/kernel/kmodule.cpp">src/kernel/kmodule.cpp</a> 
# kconfig.h
<a href="https://github.com/Andrewerr/GumOS/blob/master/src/kernel/kconfig.h">This</a> file stands for kernel configuration.<br>

|Parameter               |Description              |
|------------------------|-------------------------|
|KERENEL_VERSION         |Just kernel version.     |
|KLOOP_SLEEP_MS          |Sleep time for kernel loop( located in ```kstart(void)``` <a href="https://github.com/Andrewerr/GumOS/blob/master/src/kernel/kernel.cpp">src/kernel/kernel.cpp</a> )
|ARRAY_CAPACITY          |Default capacity for array capacity(see <a href="https://github.com/Andrewerr/GumOS/blob/master/src/kernel/types/array.h">src/kernel/types/array.h</a>)|
|HASHTABLE_CAPACITY      |Default capacity for array capacity(see <a href="https://github.com/Andrewerr/GumOS/blob/master/src/kernel/types/array.h">src/kernel/types/hashtable.h</a>)|
|TASK_STACK_DEPTH        |Max stack depth for FreeRTOS task(see https://www.freertos.org/a00125.html)|
|TIMER_SLEEP_TIME        |Timer loop sleep time.   |
|TIMER_PRIORITY          |Priority of timer task.  |
|TIMER_INITIAL_TIME      |Initial UNIX time to set.(For precision could be ```double```)|
|MICRORAMFS_SIZE         |microramfs maximum size. |
|MIN_FREE_HEAP           |This is sometimes useful to detect memory leaky. If free heap is less than MIN_FREE_HEAP then kernel will panic. |
|LOAD_GUI                |Set to 1 to enable GUI.       |
|LOAD_POWERCTL           |Set to 1 to enable powerctl.  |
|LOAD_SOUNDCTL           |Set to 1 to enable soundctl.  |
|LOAD_HIDCTL             |Set to 1 to enable keyboard.  |
|LOAD_AUTOSTART           |Set to 1 to load lua autorun
# microramfs
## Brief
microramfs is builtin virtual filesystem that store resources and some of sensors states. It has fixed maximum size and in case of exceeding this space you will get error 8 (see https://github.com/Andrewerr/GumOS/blob/master/src/kernel/error.h ).
## Initialization
microramfs initializes from static array placed in ```src/kernel/microramfs/init.h```. It could be generated from any folder with <a href="https://github.com/Andrewerr/microramfs-creator"> this </a> tool.
## Functions
Any function return ```int``` which is error code(0 means no error).<br>
Each function accepts filesystem instance(```microramfs *fs```) as first parameter.<br>


| Function                                   | Description                        |
|--------------------------------------------|------------------------------------|
|create_file(microramfs *fs,char *path)      | Creates file with given path(path should include file name).|
|write_file(microramfs *fs,char *path,uint8_t *data, uint64_t sz)| Write ```data``` to file(file will be truncated).```sz``` is size of data|
|read_file(microramfs *fs,char *path, uint8_t \**data, uint64_t *sz)|Read data from file. <br> **IMPORTANT**:```*data``` should be NULL before calling this function.<br>Size of read data will be written to ```sz```|
|rm_file(microramfs *fs,char *path)          | Remove file by given path.         |
|ls_dir(microramfs *fs, char *path, array_t \**array)| List dir and put all object instances to ```array```|
|create_dir(microramfs *fs,char *path, char *name)| Create directory with ```name``` in directory ```path```|

## Not yet implemented
Following functions are not yet implemented(and even not added to headers): ```rm_dir```, ```append_file```
# Credits
Bootlogo by <a href="https://www.flaticon.com/authors/roundicons">RoundIcons</a><br>
PlatformIO https://platformio.org<br>
FreeRTOS https://www.freertos.org<br>
m5stack https://github.com/m5stack/M5Stack<br>
## Opensource software licenses
### Lua
Files:<br>
```
src/lua
```
https://www.lua.org

```
Copyright © 1994–2019 Lua.org, PUC-Rio.
Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```

# TO-DO
A lot things to do.<br>
* Fix issues
* Add network services
* Add NTP syncrhonization
* Add bluetooth connection to smartphone for syncing messages,notifications etc.
* Make GUI
* Add sleep mode
* Add SDCard interface
* Add updater
* A lot more...<br>

