#Asterisk Opus/VP8 patch
=======================

Since Opus and VP8 cannot, as of now, be integrated in the Asterisk repositories (learn why in this [thread](http://lists.digium.com/pipermail/asterisk-dev/2013-May/060356.html)), we prepared a patch that adds support for both codecs (Opus transcoding, VP8 passthrough) to [Asterisk 11.1.2](http://downloads.asterisk.org/pub/telephony/asterisk/releases/).

##Installing the patch
To support Opus, you'll need to install [libopus](http://www.opus-codec.org/downloads/) first. No library is needed for VP8, as its support is passthrough only.

The patch was built on top of Asterisk 11.1.2: applying it on different versions may or may not work out of the box, but solving conflicts shouldn't be too hard anyway. Copy it in the Asterisk source folder and apply it:

	patch -p1 -u < asterisk_opus+vp8.diff

Run the bootstrap script to regenerate the configure:

	./bootstrap.sh

Configure the patched Asterisk.

	./configure --prefix=/usr

Make sure that codec\_opus and format\_vp8 are enabled in menuselect before going on. Besides, for better results, install the slin16 versions of the Asterisk sounds, which are not enabled by default.

	make menuselect

Compile and install.

	make
	make install

##Testing
You can test Opus using the free softphone [PhonerLite](http://phonerlite.de/download_en.htm). Make sure you choose the beta version, as the stable one does not comply with [draft-ietf-payload-rtp-opus](http://tools.ietf.org/html/draft-ietf-payload-rtp-opus-00) (RTP timestamp increment). The codec\_opus module also has a CLI command to enable debugging: type _opus set debug_ for information about it.

	Usage: opus set debug {status|none|normal|huge}
		Enable/Disable Opus debugging: normal only debugs setup and errors, huge debugs every single packet

For VP8 you can make use of the open source softphone [Linphone](http://www.linphone.org/eng/linphone/news/linphone-3.5.0-released-for-desktop.html), which added support for VP8 in version 3.5.0.

##What is missing
SDP fmtp parameters related to Opus and defined in [draft-ietf-payload-rtp-opus](http://tools.ietf.org/html/draft-ietf-payload-rtp-opus-00) are parsed but currently ignored: this means that there's no interaction between chan\_sip and codec\_opus in that sense. Besides, there still is no ad-hoc Opus configuration file for codec defaults. VP8, as anticipated, is passthrough only: besides, there's currently no way to read VP8 files for Playback.

##Help us improve the support!
Found an issue? Solved one? Added something that was missing? Help us make it better!

Developed by [@meetecho](https://github.com/meetecho)
