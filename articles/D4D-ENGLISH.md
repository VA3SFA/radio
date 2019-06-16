# D4D Installation and modification of double-sided radio stations
By cnsit (BH4DCL)
---
[D4D] (http://crkits.com/) is a low-power bilateral band radio designed by BD6CR. Its characteristics are:
- Low power, double sideband, which makes many designs of D4D possible
- One NE602 both received and sent, draining all the possibilities of this IC
- Simple and reliable 1W RF amplifier
- If you are not adding a VOX voice-activated transmitter circuit (you can use a manual switch instead), then this station can be put into a matchbox

The kits are of very high quality, and assembling, testing, and retrofitting D4D is also an interesting process. Let me summarize the transformation and production experience of the kit during the process of assembling D4D.

The original design circuit mentioned below, the relevant information can be found in [D4D Document] (https://groups.io/g/crkits/files/D4D%20Kit%20Documentations).

## Filter transformation
The original D4D filter is a 5-cell low-pass filter. The actual bandwidth of the 7MHz version is 10MHz, which is insufficient for 14MHz. By simply adding two 470p capacitors, the bandwidth can be reduced to 7.2MHz and the attenuation to 14MHz is increased by 10dB. The trade-off is that the in-band standing wave of the low-pass filter becomes larger, but this is not a problem for the D4D of the single-frequency point.
![Modified Filter Circuit] (https://github.com/va3sfa/radio/blob/D4D/articles/d4d/Image-20.png)

Two new capacitors can be mounted on the back of the board, directly in parallel with C21 and C24.
[Modified Filter Installation] (https://github.com/va3sfa/radio/blob/D4D/articles/d4d/thumbnail_Image-19.png)

The following is a comparison of the frequency response curves of the filters before and after the transformation:
![Frequency response curve comparison] (https://github.com/va3sfa/radio/blob/D4D/articles/d4d/Image-22.png)

## RF power amplifier transformation
D4D's RF power amplifier is the highlight of its design. The carefully-selected power tube can stably output 1W with the simplest circuit (measured by 12v power supply can reach 1.5W). However, in order to further improve the output signal quality and stability, the bias resistor R8 (3.3k) of the power amplifier section is connected to the base and collector of Q4.
[[RF amplifier conversion circuit] (https://github.com/va3sfa/radio/blob/D4D/articles/d4d/Image-23.png)
[Renovation and Installation of RF Power Amplifier] (https://github.com/va3sfa/radio/blob/D4D/articles/d4d/Image-16.png)

## VOX voice control transformation
The DOX's original VOX voice control circuit requires at least 1.2Vpp (600mVcp) of audio signal to trigger. For many laptops and even desktop sound cards, this output is quite difficult. The BD6CR even recommends using an external sound card to achieve this trigger level.

However, the D4D's VOX preamp can be completely modified from a switching circuit to an amplifying circuit, greatly reducing the need for computer sound card output levels.
![VOX Retrofit Circuit](https://github.com/va3sfa/radio/blob/D4D/articles/d4d/Image-21.png)
After testing, this modified circuit can be reliably triggered at 400~3KHz with 400mVpp (200mVcp) audio. The reason why the sensitivity is not adjusted higher is to avoid false triggering. In the place where the mains interference is serious, the trigger may trigger the launch state due to the touch of the human body.
![VOX Retrofit Installation] (https://github.com/va3sfa/radio/blob/D4D/articles/d4d/Image-18.png)
The parameters of the voice control circuit components are as follows:
- R1 14.3
- C1 2u
- R2 1k
- R3 2k (the original R15 position)
- R4 10k
- D1 1N4148 (You can use the original D2, because Q5 does not work in the switch state, so it is not protected by D2)
- R5 20k
- C2 0.1u
According to the above component parameters, the VOX voice control circuit at this time:
- Trigger delay 10mS
- Cut off delay 15mS
Modify the values ​​of R5 and C2 to adjust the delay time. Note, however, that R5 cannot be less than 10k.

## Start-up of short-time launch
The original design of D4D also has a "characteristic" by design: the moment when the power is turned on, it will immediately enter the transmitting state, and it will automatically switch back to the receiving state after about 0.7 seconds.

The cause of this phenomenon is still uncertain. Some people think that VOX's audio input capacitor is short-circuited to ground when it is turned on. However, when I installed D4D, the VOX part was installed before the peripheral circuit such as NE602. At that time, the power-on was normally entered into the receiving mode, and there was no short-term switching of the transmitting state. Therefore, it can only be said that the cause is unknown.

This phenomenon has not been a big problem. However, if your antenna system has a high standing wave, then the RF interference at the moment of the power-on will make the D4D unable to return to the receiving state, thus “locking” to the transmitting state, which is not fun. Therefore, it must be solved.

The solution is also very straightforward: the power to the transceiver is disconnected at the moment of power-on, so that even if VOX is triggered for some reason, the relay will not operate. Wait a moment, wait until the VOX circuit is stable, and then automatically return the relay.
![Reform Circuit of Boot Relay Delay] (https://github.com/va3sfa/radio/blob/D4D/articles/d4d/Image-24.png)
The circuit component selects the chip component, which can be installed in the middle of the front R6 and Q6 of the circuit board, and cuts the circuit which is originally connected to the relay coil into the delay circuit.

![Renovation and installation of the boot relay delay] (https://github.com/va3sfa/radio/blob/D4D/articles/d4d/Image-17.png)

The component parameters of the relay delay circuit are as follows:
- P-MOSFET: AO3407
- C1: 0.33u (I don't have a chip capacitor of this capacity, so I used 1u. The disadvantage is that the discharge time is long. If the power is turned on within 5 seconds after shutdown, the delay will be invalid, but the problem is not big, and it will rarely switch quickly. machine)
- R1: 1M
After the delay circuit is added, there is no need to worry about the boot cut.

## Other possible modifications
- A 1N4148 is connected to the power supply part of the NE602 to reduce the operating voltage to 4v, which can further improve the carrier rejection ratio and reduce the spurious output.
- Add further attenuation to the audio input to reduce harmonics and improve output signal quality. When the audio signal is actually input at 600mVpp, the output is up to 1.5W (12V, 320mA), which has a reduced space.

## Problems needing attention during use
- The supply current must be sufficient (greater than 400mA) to avoid relay switching back and forth when switching between transceivers

In general, the D4D design is simple, the kit is of good quality, and the production is very interesting and convenient!