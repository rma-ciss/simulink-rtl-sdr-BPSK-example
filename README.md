simulink-rtl-sdr-BPSK-example
=============================

This git repository contains the results of a lab-session that was done at the Royal Military Academy in the scope of the courses on "Digital Communications" in May 2013. 

The aim of the lab-session was to implement in Simulink a complete BPSK receiver (optimal filter - carrier synchronisation- symbol timing synchronisation - demodulation - voice decoding), in order to receive and decode in real time a wireless voice transmission.

The receiver is based on a DVB-T dongles with an embedded Realtek RTL2832U chip-set and the Simulink-RTL-SDR project that was initiated at the Communication Engineering Lab (CEL) at the Karlsruhe Institute of Technology (KIT), Germany, http://www.cel.kit.edu.


General description
===================
For the lab-session, the students received the Simulink model of the transmitter (the easiest part of the wireless communication chain :-)) and the AWGN channel model block. The voice is coded using a normal delta modulator at 16 kbits/s. In a delta modulator, the voice is coded with 1 bit/Sample. This gives several advantages 
* At the transmitter, the coded voice does not need to be packeted and put in a frame. 
* At the receiver side , there is no need to implement a frame synchronisation, so the students could realy consentrate on the PHY layer issues (carrier and symbol synchronisation).
* In the BPSK demodulator, there is no need to correct for the 180°-phase ambiguity

In a first step, the student were asked to design and implement the BPSK receiver in a Simulink model, but without hardware in the loop. For this, they could make use of the delta modulator block and a simple AWGN channel model block that also introduced a frequency, phase and timing offset. The final model of this first part can be found in the file <color blue>Tx_Rx_BPSK_delta_simulatie.mdl</color>

In a second step, the student were asked to integrate the hardware in the model (USRP 1 at the Tx side and a DVB-T dongle at the Rx side). The result of this part can be found in the files <color blue>Tx_BPSK_delta_USRP.mdl</color> and <color blue>Rx_BPSK_delta_RTL_SDR.mdl</color>

The transmitter
===============
In the transmitter, the application data is taken either from the sound card or from a sinus generator at 16 kbit/s in frames of 256 Samples/frames. The application data is coded by a deltamodulator, mapped on a BPSK constellation and filtered by a RRC pulse shape filter. The filter will upsample the signal by 16 (16 Samples/ symbol). Before outputting to the USRP1, the IQ samples are multiplied by 2^14, to make use of the full range of the USRP DAC. The transmit frequency is at 433.92 MHz (in the middle off the license free ISM band)

The receiver
============
In the receiver, the IQ samples are taken from the DVB-T dongle at 1MS/s. By careful, this is the lowest sample frequency that can be set in the hardware. Because 1MS/s is too much for our application, the signal is decimated by 4 in software. At this level, the signal is at 16 Samples/symbol. After the matched filter, a coarse   offset estimation and frequency wipe-off is performed based an the delay-and-multiply method. In the next block, the symbol timing recovery will be performed, based on a block of the Simulink communication blockset. At the output of this block, the signal is at 1Sample/symbol. The nect block will perform a fine frequency and phase recovery based on a Costas loop. After the Costas loop, the demodulation, decision and voice decoding is performed. The decoded data is then send to the soundcard.


Real-time issues
================
In this implementation, we noticed some problem due to real-time issues, so the use of fast computers is advised. The model can also be accelerated by leaving out the plots (less didactic, but you have no choise) and by making use of the accelerator in the Simulink simulator environment.
