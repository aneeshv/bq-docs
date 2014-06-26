Device Driver
*************
Latest Linux device driver for bq27xxx single-cell gauges is available here:
https://github.com/aneeshv/linux-bq27xxx.git
branch: master

Support for ROM-based Gauges (bq274xx/bq27621):
===============================================
The above driver supports some of the special requirements for ROM-based
gauges such as bq27421 and bq27621.

What it does:
In addition to the regular reporting functions (Voltage(), Current(), SOC()
etc.) this driver has APIs for reading and writing the DM of the gauge.
These APIs are in turn used for initializing the DM of ROM based gauges.
The initialization happens at startup(about ~65s into the the startup).
After startup we poll the status of ITPOR every 6 minutes and repeat the
DM initialization if it’s set. The polling happens along with the polling
of other parameters such as Current(), SOC() etc and the period for this
polling loop is customizable. The registers to initialized in the DM may
be specified in the code in an array like this:

static struct dm_reg bq276xx_dm_regs[] = {
	{64, 2, 1, 0x2C},	/* Op Config B */
	{82, 0, 2, 1000},	/* Qmax */
	{82, 2, 1, 0x81},	/* Load Select */
	{82, 3, 2, 1340},	/* Design Capacity */
	{82, 5, 2, 3700},	/* Design Energy */
	{82, 9, 2, 3250},	/* Terminate Voltage */
	{82, 20, 2, 110},	/* Taper rate */
};

What it doesn’t:
It doesn’t have a mechanism to save and restore Ra values and Qmax as
suggested in the Quick Start Guide.  This means it will reload default
values every time the battery is inserted.

bqtool
******
bqtool is an Android commandline utility for programming, monitoring,
and debugging TI's bq27xxx family of single-cell gas gauges. Although
it's created for Android it can be easily ported for any Linux system.

It's available here:
https://github.com/aneeshv/bqtool.git
branch: master

Commands supported:
--flash :
	bqtool --flash <bqfs file|dffs file>
	Eg: bqtool --flash /system/bin/bq27520_v302.bqfs

For more details see bqtool's README:
https://github.com/aneeshv/bqtool/blob/master/README
