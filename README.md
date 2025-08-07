
# LOLSpoof

LOLSpoof is a an interactive shell program that automatically spoof the command line arguments of the spawned process.
Just call your incriminate-looking command line LOLBin (e.g. `powershell -w hidden -enc ZwBlAHQALQBwAHIAbwBjAGUA....`) and LOLSpoof will ensure that the process creation telemetry appears legitimate and clear.


<img width="1997" height="1607" alt="LOLsp00f" src="https://github.com/user-attachments/assets/3414162a-029c-4ce5-ad0b-92033fcbc5d6" />


> Use only for 64-bit LOLBins

## Why
Process command line is a very monitored telemetry, being thoroughly inspected by AV/EDRs, SOC analysts or threat hunters.

## How
1. Prepares the spoofed command line out of the real one: `lolbin.exe " " * sizeof(real arguments)`
2. Spawns that suspended LOLBin with the spoofed command line
3. Gets the remote PEB address
4. Gets the address of RTL_USER_PROCESS_PARAMETERS struct
5. Gets the address of the command line unicode buffer
6. Overrides the fake command line with the real one
7. Resumes the main thread

## Opsec considerations
Although this simple technique helps to bypass command line detection, it may introduce other suspicious telemetry:
1. Creation of suspended process
2. The new process has trailing spaces (but it's really easy to make it a repeated character or even random data instead)
3. Write to the spawned process with WriteProcessMemory

## Build
Built with Nim 1.6.12 (compiling with Nim 2.X yields errors!)

`nimble install winim`

## Known issue
Programs that clear or change the previous printed console messages (such as `timeout.exe 10`) breaks the program. when such commands are employed, you'll need to restart the console. 
Don't know how to fix that, open to suggestions.
