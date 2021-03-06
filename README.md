rtk-streamserver
================

Modified from str2str, a CUI AP of RTKLIB 2.4.2. The strsvrstat loop in the main function was modified by ddding some code to update the system time. Thus every time when the strsvrstat ( show stream server status ) executed, the GPS time in the GPS raw message will be extracted and be utilized to update the system time. If the difference between GPS time and system time is not big ( er than 1sec ), the system time will not update.

Details:
(1) For S1315F receiver support
modified decode_stqtime function in skytraq.c file. This function decodes the gps time info, a line "TimeStamp = raw->time" is added at the end of this function.
(1) For RTCM3 support
modified decode_type1019 function in rtcm3.c file. This function decodes the gps ephemerides, a line "TimeStamp = rtcm->time" is added at the end of this function.
(3) For uBlox support
modified decode_rxmraw function in ublox.c file. This function decodes the ubx raw measurement data, a line "TimeStamp = rtcm->time" is added at the end of this function.

Compile Note:
if define the macro TSDEBUG, then the unixtime acquired will print every 5 sec.

Usage example:
(1) For S1315F-Raw receiver
./str2str -in serial://ttyUSB0:115200:8:n:1:off#stq -out ntrips://:transient@192.168.10.110:2101/BASE1 -out serial://ttyS1:115200:8:n:1:off#rtcm3
or
./str2str -in serial://ttyUSB0:115200:8:n:1:off#stq -out ntrips://:transient@192.168.10.110:2101/BASE1#rtcm3
(2) For OEMV2 receiver
./str2str -in serial://ttyUSB0:115200:8:n:1:off#rtcm3 -out ntrips://:transient@192.168.10.110:2101/BASE1#rtcm3
(3) For ublox receiver
./str2str -in serial://ttyUSB0:115200:8:n:1:off#ubx -out ntrips://:transient@192.168.10.110:2101/BASE1#rtcm3

The last -out option is used to make sure this program execute the strconv function, which contains the time extracting routine, since this function only executes when message format conversion is needed.
