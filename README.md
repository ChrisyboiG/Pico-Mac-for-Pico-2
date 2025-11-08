# Pico-Mac-for-Pico-2
This repository provides a guide on compiling the Pico Mac project by TechByAndroda for the Raspberry Pi Pico 2.  

This is a guide for Linux, if you are using Windows then it is recommended that you use WSL.  

This project requires Pico SDK to work, use this repository to find how to install it.  
https://github.com/raspberrypi/pico-sdk  

To compile Pico Mac for Pico 2 first clone the repository:  
```
git clone https://github.com/TechByAndroda/pico-mac.git --recursive  
```
After cloning the repository, then copy your Mac Plus V3 ROM to the pico-mac folder and name it "Mac.ROM"  
Note: I do not endorse piracy, please aquire ROM from a legal source, like using CopyRoms.  

Decide how much RAM you want to have in your virtual Mac, any amount between 128MB and 420MB should be fine.  
Then replace the "REPLACE" text in the make command with your choice of RAM.  

Then build Umac:  
```
cd pico-mac  
cd external/umac  
make MEMSIZE=REPLACE DISP_WIDTH=640 DISP_HEIGHT=480  
cd ../..  
./external/umac/main -r 'Mac.ROM' -W rom.bin
```

After building Umac:  

Note: Replace the "REPLACE" text in the cmake command with your RAM size from earlier.  
```
mkdir incbin  
xxd -i < rom.bin > incbin/umac-rom.h
```
```
# When using an internal disc image:  
# Also if using an internal disc image then remove the -DUSE_SD=true part of the cmake command coming up. You will need to place your disc.bin into the pico-mac folder.  
# Note: I haven't actually used this method so I don't have all the info required for how to get this disc.bin file.  
xxd -i < disc.bin > incbin/umac-disc.h  
# When using an SD card for disc image:  
echo > incbin/umac-disc.h  
```
```
mkdir build  
cd build  
cmake .. -DMEMSIZE=REPLACE -DUSE_SD=true -DUSE_VGA_RES=1 -DPICO_BOARD=pico2  
cd ..  
make -C build  
```
If all goes well then you should have a firmware.uf2 file in the "build" directory that you can flash to the Pico 2.  

When using an SD card make sure that it is formatted as FAT, not FAT32. Drag your Mac OS .img to the root of the SD card and name it "umac0.img".  

This may provide extra info on how to get a Mac OS img file: https://jcm-1.com/product/picomicromac/  
