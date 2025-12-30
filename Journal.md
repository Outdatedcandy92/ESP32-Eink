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