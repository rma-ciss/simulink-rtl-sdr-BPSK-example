simulink-rtl-sdr-BPSK-example
=============================

This git repository contains the results of a lab-session that was done at the Royal Military Academy in the scope of the courses on "Digital Communications" in May 2013. 

The receiver is based on a DVB-T dongles with an embedded Realtek RTL2832U chip-set and the Simulink-RTL-SDR project that was initiated at the Communication Engineering Lab (CEL) at the Karlsruhe Institute of Technology (KIT), Germany, http://www.cel.kit.edu.


General description
===================

The aim of the lab-session was to implement in Simulink a complete BPSK receiver (optimal filter - carrier synchronisation- symbol timing synchronisation - demodulation - voice decoding), in order to receive and decode in real time a wireless voice transmission.

For the lab-session, the students received the Simulink model of the transmitter (the easiest part of the wireless communication chain :-)) and the AWGN channel model block. The voice is coded using a normal delta modulator at 16 kbits/s. In a delta modulator, the voice is coded with 1 bit/Sample. This gives several advantages 
* At the transmitter, the coded voice does not need to be packeted and put in a frame. 
* At the receiver side , there is no need to implement a frame synchronisation, so the students could realy consentrate on the PHY layer issues (carrier and symbol synchronisation).
* In the BPSK demodulator, there is no need to correct for the 180°-phase ambiguity

In a first step, the student were asked to design and implement the BPSK receiver in a Simulink model, but without hardware in the loop. For this, they could make use of the delta modulator block and a simple AWGN channel model block that also introduced a frequency, phase and timing offset. The final model of this first part can be found in the file Tx_Rx_BPSK_delta_simulatie.mdl

In a second step, the student were asked to integrate the hardware in the model (USRP 1 at the Tx side and a DVB-T dongle at the Rx side). The result of this part can be found in the files Tx_BPSK_delta_USRP.mdl and Rx_BPSK_delta_RTL_SDR.mdl

The transmitter
===============



The receiver
============


Real-time issues
================
