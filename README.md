# cast_xmr

Cast XMR - high speed CryptoNight miner for Radeon RX Vega GPUs

Here is 'Cast XMR' the high performance CryptoNight miner for CryptoNote based crypto currencies like [Monero (XMR)](https://getmonero.org/), [Bytecoin (BCN)](https://bytecoin.org), [DigitalNote (XDN)](http://digitalnote.org) and many more. 

Cast XMR is specially optimized for the Radeon RX Vega series of GPUs, reaching hash rates of more then 2000 CryptoNight Hashes/s on an single RX Vega 56 or Vega 64.


## Features

- Full support for CryptoNight/CryptoNote based currencies:
  - **CryptoNightV7 (CNv1)**
	- [Monero (XMR)](https://getmonero.org)
	- [Lethean (LTHN) (former Intense)](https://intensecoin.com)
	- [Graft (GRFT)](https://www.graft.network)
  - **CryptoNightV8 (CNv2)**
	- [Monero (XMR) (upcoming V8 network upgrade)](https://getmonero.org)
  - **CryptoNight (Classic)**
	- [Electroneum (ETN)](https://electroneum.com)
	- [Bytecoin (BCN)](https://bytecoin.org)
	- [DigitalNote (XDN)](http://digitalnote.org)
	- [LeviarCoin (XLC)](https://leviarcoin.org)
	- [Karbo (KRB)](https://karbo.io)
  - **CryptoNightXTL**
	- [Stellite (XTL)](https://stellite.cash)
  - **CryptoNight Heavy**
	- [Loki (LOKI)](https://loki.network)
	- [Saronite (XRN)](https://saronite.io)
  - **CryptoNightXHV Heavy**
	- [Haven (XHV)](https://havenprotocol.com)
  - **CryptoNightV7 Lite**
	- [Aeon (AEON)](https://www.aeon.cash)
	- [Turtlecoin (TRTL)](https://turtlecoin.lol)
  - **CryptoNightTUBE Heavy**
	- [BitTube (TUBE) (former IPBC)](https://coin.bit.tube)
  - **CryptoNight Fast**
	- [Masari (MSR)](https://getmasari.org)
  - **CryptoNightFEST**
	- [Festival Coin (FEST)](https://festivalcoin.net)


- Fastest miner for AMD Radeon RX Vega GPU series
- Support for following GPUs:
	- Radeon RX Vega 64 
	- Radeon RX Vega 56
	- Radeon Vega Frontier Edition
	- Radeon RX 480/RX 580 
	- Radeon RX 470/RX 570 
- Multiple GPU support
- Monitor temperature and fan speed of each GPU
- Full pool support
- Fast Job Switch option to minimize outdated shares
- Nicehasher support
- Remote http access for statistics in JSON format 
- Rate Watchdog to monitor each GPU performance
- Includes **[switch-radeon-gpu command line tool](http://www.gandalph3000.com/cast_xmr/switch-radeon-gpu-compute-mode-hbcc-largepages/)** to restart GPUs, switch on/off HBCC, Compute Mode and Large Pages support for Radeon GPUs

## Requirements

- Windows 8/8.1/10 64 bit
- For about **50% higher** hash rates the [Radeon Driver 18.3.4](https://support.amd.com/en-us/kb-articles/Pages/Radeon-Software-Adrenalin-Edition-18.3.4-Release-Notes.aspx) or [Radeon Driver 18.5.1](https://support.amd.com/en-us/kb-articles/Pages/Radeon-Software-Adrenalin-Edition-18.5.1-Release-Notes.aspx) or later has to be installed as includes profound performance improvements over older drivers.


## How To

cast_xmr has a command line interface. For a minimal configuration run:

``
cast_xmr -S [pool server] -u [username or wallet address] --algo=[n]
``

The <code>--algo</code> option specifies which CryptoNight variant to use:

 - <code>--algo=0</code> for CryptoNight (Classic)
 - <code>--algo=1</code> for CryptoNightV7 (CNv1)
 - <code>--algo=2</code> for CryptoNight-Heavy
 - <code>--algo=3</code> for CryptoNight-Lite
 - <code>--algo=4</code> for CryptoNightV7-Lite
 - <code>--algo=5</code> for CryptoNightTUBE-Heavy
 - <code>--algo=6</code> for CryptoNightXTL
 - <code>--algo=7</code> for CryptoNightXHV-Heavy
 - <code>--algo=8</code> for CryptoNight-Fast
 - <code>--algo=9</code> for CryptoNightFEST
 - <code>--algo=10</code> for CryptoNightV8 (CNv2)

If algo is not specified or set to -1 the correct one for mining Monero will be selected.

To select which GPU to use the <code>-G</code> switch, e.g. for using the 2nd card use:

``
cast_xmr -S [pool server] -u [username or wallet address] -G 1
``

To select multiple GPU to use the <code>-G</code> switch and list comma separated the GPUs which should be used, e.g. for using the 1st and 3rd card use:

``
cast_xmr -S [pool server] -u [username or wallet address] -G 0,2
``


In case you have multiple OpenCL implementation installed or mixed GPUs (Nvidia, Intel, AMD), the correct OpenCL implemenation can be selected with the "--opencl" switch:

``
cast_xmr -S [pool server] -u [username or wallet address] --opencl 1 -G 0
``

For a complete list of configuration options run:

``
cast_xmr --help
``


### Optimization Options

#### <code>--intensity</code>

With the <code>--intensity</code> the memory allocation of the card can be optimized. If not specified an intensity will be selected which should give under most circumstances a stable hash rate. Depending on many factors (no monitor attached to card, HBCC turned off) the intensity can be increased to reach higher hash rates.

Intensity can be set in the range from 0 to 10 (on some GPUs upto 12). It can be specified for each GPU separately by separating the values by comma. To set GPU0 to intensity 10, GPU1 to default intensity (-1) and GPU2 to intensity 7 following can be specified: 

``
cast_xmr -G 0,1,2 --intensity=10,-1,7 (... rest of configuration ...)
``

Note: If the intensity is set to high the hash rate can decrease, so higher is not always better. Also the stability can decrease with higher intensity, observed as a sharp drop of the hash rate after a few minutes running.


#### <code>--fastjobswitch</code>

As the processing is done in a large batch in parallel a processing round trip can take a few seconds. In cases where the pool sends a new job the processing time until the batch ends is lost and will generate 'Outdated Shares'. To prevent this add the <code>--fastjobswitch</code> to immediately switch to the new job. The displayed hash rate can fluctuate more then without the option so enable after finding your desired intensity and settings. Depending on the rate of job changes the net hash rate can increase up to 10% as nearly no 'Outdated Shares' will be produced anymore.

#### <code>--ratewatchdog</code>

Add the <code>--ratewatchdog</code> option to let Cast XMR monitor the hash rate of the GPU, if a sustained drop in hashrate is detected the effected GPU will be restarted. 




More info at http://www.gandalph3000.com

