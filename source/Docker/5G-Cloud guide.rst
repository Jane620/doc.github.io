



Construction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
AirFramework : CU
ASIK + 2 ABIL : DU
eNB :





ASIK
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
If ASIK type is A101 HW, this steps should be followed:

plug out ABIL, only keep ASIK in AMIA, if NOT, ABIL loner upgrade will be failed
plug cable directly between local PC and ASIK LMP, config local PC IP address, such as: 192.168.255.126/255.255.255.0
enable SSH access: visit WebUI by default LMP IP, such as: https://192.168.255.129
yaft new version to ASIK before config ASIK LMP IP, and update YAFT tool to the newest first
check new version has beem yaft to both /ffs/fs1 and /ffs/fs2, you can yaft twice to ensure it.
modify ASIK LMP IP on /ffs/fs1/ip-config  and /ffs/fs2/ip-config
crasign /ffs/fs1/ip-config  and /ffs/fs2/ip-config
reboot ASIK
