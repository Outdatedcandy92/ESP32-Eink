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

