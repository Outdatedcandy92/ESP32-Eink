## 29/12/25

Saw this really [cool thing](https://www.reddit.com/r/esp32/comments/1kgrwxk/minimal_github_commit_tracker_using_an_esp32_and/) on reddit a while ago and I knew right at that moment that I had to make one for myself.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/0bebac312c9819ce_y9vexikHeh01AAAAABJRU5ErkJggg__)

It's basically just an E-Ink display with an ESP32 on the back; seems pretty simple to make at first glance;  and so I started my journey.

I started off by just searching what main MCU I want to use and eventually ended up selecting the ESP32-C3 as it can be programmed over USB, has 4mb of internal flash, and is very power efficient. It had everything I needed!

Moving on to designing the actual schematics now. Espressif provides nice [hardware design guidelines](https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32c3/index.html) for the C3 chips so that's what I've referred to for this project.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/113b59d1c3005683_veM6mjE0Zm97mZd2u70cAAABo4VdnAAAAqglfAAAAqglfAAAAqt1wWBazvdMjqAAAAABJRU5ErkJggg__)

This was my minimal schematic for the ESP32-C3 It's got some power filtering for the VDD3P3, and got an RF matching circuitry in which for now I've just used the recommended values for the matching circuitry provided in the guide (I will most likely change this later).

Now after setting up the minimal schematics I though I was actually cooked because I needed 4 free GPIO pins for SPI and I initially thought I could not use the JTAG pins as GPIO, so yeah I uh basically just gave up for 5 minutes and then realized that these pins can have an alternate function of GPIO so I was safe; I could actually assign SPI on those pins and drive the E-Ink display.

Oh and I also decided to add an external clock source for the RTC clock to decrease average power consumption. I figured that since I'm going to be putting the ESP32 in deep sleep most of the time, having an external crystal would be helpful.

For my power I decided to use a buck boost convertor because I'm actually planning to power this from a battery and would like for it be as efficient as it possibly can. I used the TPS63001 as the chosen convertor as it's high efficiency and tiny.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/4bb52e91faeeab93_P8Bs8nHytIeXj8AAAAASUVORK5CYII_)


### Time Spent: 2.5 Hours

--- 

## 30/12/25

I added circuitry for driving an E-ink display today. For this I mainly referred to a few different websites; some of which are [adafruit](https://cdn-learn.adafruit.com/assets/assets/000/127/535/original/eink___epaper_schem.png?1707320728), [waveshare](https://www.waveshare.com/w/upload/5/57/2.13inch_e-Paper_HAT_Schematic.pdf),[alibaba page](https://www.alibaba.com/product-detail/E-paper-Technology-2-13-Inch_1601018719280.html) (for pinout and information about drivers).

In the end I ended up with the circuitry down below; looks pretty complex but all it really does is take 3.3v In and boosts it using the mosfet circuitry you see on the left side to power itself up.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/eeeeb1ea157bcba2_DzvpYL3CFZykAAAAAElFTkSuQmCC)

*final schematic*

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/b4999551a67e8722_A9ZellqtghfwAAAAAElFTkSuQmCC)

Moving on I assigned everything temporary footprints (I hate assigning footprints), went with 0402s components and moved onto the actual PCB design.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/d0bb86fe26f8f7b6_zw24T4mFwIYAAAAASUVORK5CYII_)

I started off with assigning some net classes and color coding them to get a better visual 

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/48e8921263174a7e_weXqATc5st4jwAAAABJRU5ErkJggg__)

I then drew a rectangle using the dimensions of the E-Ink display just to get a sense of scale and see how I should place everything. (I didn't make a board edge layer yet)

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/eb8321d017807c6e_8TyG2rAAAAAElFTkSuQmCC)

Started off with laying out the E-Ink circuitry as it was pretty easy

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/1ece0b861686e43e_O_nE4to6JmwNSfpAPRXueuDkuWGYionTHYIaIiIgkjWUmIiIikjQGM0RERCRpDGaIiIhI0hjMEBERkaQxmCEiIiJJYzBDREREksZghoiIiCSNwQwRERFJGoMZIiIikjQGM0RERCRpDGaIiIhI0hjMEBERkaQxmCEiIiJJYzBDREREkpbQYOZ7T31J3EQB8DwtjecoPDxP4eF5WhrP0dLuabwN33vqS7in8TbxJvKRjGtJhrwqt7iRiIiISCoSmpkhIiIiSjQGM0RERCRpDGaIiIhI0hjMEBERkaQxmCEiIiJJYzBDREREksZghoiIiCSNwQwRERFJGoMZIiIikjQGM0RERCRpDGaIiIhI0hjMEBERkaQxmCEiIiJJYzBDREREksZghoiIiCSNwQwRERFJGoMZIiIikjQGM0RERCRpDGaIiIhI0hjMEBERkaQxmCEiIiJJYzBDREREksZghoiIiCSNwQwRERFJGoMZIiIikjQGM0RERCRpDGaIiIhI0hjMEBERkaQxmCEiIiJJYzBDREREksZghoiIiCSNwQwRERFJGoMZIiIikjQGM0RERCRpDGaIiIhI0hjMEBERkaQxmCEiIiJJYzBDREREksZghoiIiCSNwQwRERFJGoMZIiIikjQGM0RERCRpDGaIiIhI0hjMEBERkaQxmCEiIiJJYzBDREREksZghoiIiCSNwQwRERFJ2v8PjC3LiLRsbUsAAAAASUVORK5CYII_)

And then moved onto the ESP32 which was way harder

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/bfbe93d9f6c22295_0AAAAAElFTkSuQmCC)

The ESP32 pinout is questionable I would say. The 40Mhz crystal is like pretty close to the LNA pin and fitting the decoupling capacitors close is really hard when they are in 0402 package; which is why I'm considering switching to 0201s. But that's a job for tomorrow as I was pretty tired at this point.

Here's how my final PCB look at the end of today

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/af4cad40c90a623d_w8NrOnxAbHMCgAAAABJRU5ErkJggg__)


### Time Spent: 3 Hours

---
## 10/01/26 and 11/01/26

FYI this journal entry is for what I've done yesterday and today.


I first started off by adding a LiPo charger IC on board, I did some research and ended up using the **BQ24074R**. To be honest I spent way to much time reading the datasheet to figure out how to configure this thing properly.

I wanted to set the current input limit to 500mA because I plan on powering it from USB 2.0 sources, so I connected my EN pins appropriately to set USB500 mode.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/b3eb2772d0334ea8_xnFJgAAAAASUVORK5CYII_)


Other configurations were easy and pretty straightforward, I used the formula given by TI to set the fast charge current to 0.5A or 500mA.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/588f3bc83b388b29_bb8OZAAAAAAG_Vvf4yuLGBF9AAAAGBF9AAAAGBF9AAAAGBF9AAAAGBF9AAAAGBF9AAAAGBF9AAAAGBF9AAAAGBF9AAAAGBF9AAAAGBF9AAAAGBF9AAAAGBF9AAAAGBF9AAAAGBF9AAAAGAm9CwKLbYXPMgAAAABJRU5ErkJggg__)


But the ILIM pin is was confused me a lot, because in the datasheet it said to never leave it floating, where as since I'm in USB500 mode it shouldn't matter and that's what a design reference said.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/310d8440ca77a423_ILfeJalbpZgAAAABJRU5ErkJggg__)

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/dc6a2df1f954add2_Is7oXTqZInQAAAAASUVORK5CYII_)


So I spent a good 30 minutes trying to figure this out but in the end I gave up and just connected a resistor going to ground from ILIM anyways, I figured let's just be safe rather than sorry later.


![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/440f6f986a3e4369_9NmfZH3UyHskyHskyHskyL8kyHskyHsmz3fzzKrNXky9wSAAAAAElFTkSuQmCC)


After finishing up my schematics I moved onto the PCB layout.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/98ffee03d88eb445_CM5TvpBa6JsAAAAASUVORK5CYII_)

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/8ba3fd36deb3c5ed_AfUgWMfKjtTEAAAAAElFTkSuQmCC)

Now routing was relatively straightforward for this board. The only place where I needed to put some effort was the LNA line, where I had to impedance match it to 50 Ohms. For this I referred to JLCPCB's Impedance calculator and found what trace width I would need with a specific board stackup.

I went with **JLC04161H-3313** stackup as it doesn't require any extra charge and It has a reasonable trace width for a 50 Ohms microstrip.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/2c533a783b6857bb_8QcCIAACIAACIAACIAACIAACthJwkk7MVRenCLNcdUQaCIAACIAACIAACIAACIAACLiaAISZqy8vGgcCIAACIAACIAACIAACICACAQgzEa4S6ggCIAACIAACIAACIAACIOBqAhBmrr68aBwIgAAIgAAIgAAIgAAIgIAIBCDMRLhKqCMIgAAIgAAIgAAIgAAIgICrCUCYufryonEgAAIgAAIgAAIgAAIgAAIiEIAwE_EqoY4gAAIgAAIgAAIgAAIgAAKuJgBh5urLi8aBAAiAAAiAAAiAAAiAAAiIQADCTISrhDqCAAiAAAiAAAiAAAiAAAi4mgCEmasvLxoHAiAAAiAAAiAAAiAAAiAgAgEIMxGuEuoIAiAAAiAAAiAAAiAAAiDgagIQZq6_vGgcCIAACIAACIAACIAACICACAT_H_mo6VuZkIGqAAAAAElFTkSuQmCC)

I set up my a net class for the antenna traces to follow the required width and while routing out the antenna lines I decided to make use of the kicad RF-Tools plugin. I used it to round my tracks and also add fencing vias.
I had to do a bit of math to get the offset for fencing vias. To get the distance between the vias, I used the formula:

distance between vias = wavelength/8

I used Saturn PCB toolkit to calculate the wavelength based on the frequency and dielectric constant.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/97c3403e07e1253e_n9RPH3Jg0MVsQAAAABJRU5ErkJggg__)

I set my via fence according to the calculations 

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/bce1485ec65f5919_zP8LoO8wbiWAAAAAElFTkSuQmCC)



Here's how the rest of my board looked when I finished routing

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/3d5cd7a892209b1a_Dz49RWzSj5heAAAAAElFTkSuQmCC)

I decided to not use these square test pads and instead add footprint for Keystone's test points, which are cool little things which can basically plug into the hole and then you can attach a test probe onto them. No soldering or anything is required.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/672ce6f203cdd784_H8aRuSOgI3nygAAAABJRU5ErkJggg__)
![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/d942ccac0d9e673f_cgfa86LO9iwAAAAASUVORK5CYII_)

The PCB was finished at this point, all I needed to do was assign component values. And so I spent like the next 30 minutes copy pasting LCSC part numbers into KiCad.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/82e01e626d524e65_wFay5edGCrj_AAAAABJRU5ErkJggg__)

I also made a few changes such as switching to a smaller footprint crystals, and getting a better inductor with minimal DCR. This caused a few footprints to change, so I quickly had to reroute a few things.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/242449d8e7afc8a2_AQgbDqWVyNhUAAAAAElFTkSuQmCC)


This was pretty much it for this project, after finishing the PCB I spent some time on LCSC making a cart and then setting up the repository for this project.


## Time Spent: 