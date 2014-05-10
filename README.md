#Asterisk Opus patch
=======================

Since Opus cannot, as of now, be integrated in the Asterisk repositories (learn why in this [thread](http://lists.digium.com/pipermail/asterisk-dev/2013-May/060356.html)), we prepared a patch that adds support for Opus transcoding to [Asterisk 12.2.0](http://downloads.asterisk.org/pub/telephony/asterisk/releases/).

##Installing the patch
To support Opus, you'll need to install [libopus](http://www.opus-codec.org/downloads/) first.

The patch was built on top of Asterisk 12.2.0: applying it on different versions may or may not work out of the box, but solving conflicts shouldn't be too hard anyway. Copy it in the Asterisk source folder and apply it:

	patch -p1 -u < asterisk_12.2.0_opus.diff

Run the bootstrap script to regenerate the configure:

	./bootstrap.sh

Configure the patched Asterisk.

	./configure --prefix=/usr

Make sure that codec\_opus is enabled in menuselect before going on.

	make menuselect

Compile and install.

	make
	make install

##Testing
You can test Opus using the free softphone [PhonerLite](http://phonerlite.de/download_en.htm). Make sure you choose the beta version, as the stable one does not comply with [draft-ietf-payload-rtp-opus](http://tools.ietf.org/html/draft-ietf-payload-rtp-opus-00) (RTP timestamp increment). The codec\_opus module also has a CLI command to enable debugging: type _opus set debug_ for information about it.

	Usage: opus set debug {status|none|normal|huge}
		Enable/Disable Opus debugging: normal only debugs setup and errors, huge debugs every single packet

##What is missing
SDP fmtp parameters related to Opus and defined in [draft-ietf-payload-rtp-opus](http://tools.ietf.org/html/draft-ietf-payload-rtp-opus-00) are parsed but currently ignored: this means that there's no interaction between chan\_sip and codec\_opus in that sense. Besides, there still is no ad-hoc Opus configuration file for codec defaults.

##Help us improve the support!
Found an issue? Solved one? Added something that was missing? Help us make it better!

Developed by [@meetecho](https://github.com/meetecho)
