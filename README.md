# integrated headphone-simulate-stereo-speakers

feteched from `xinleio/Headphones-simulate-stereo-speakers` virtual stereo settings and using 
`jaakkopasanen/AutoEQ` headphone/earpods equalization project

* [Headphones-simulate-stereo-speakers](https://github.com/xinleio/Headphones-simulate-stereo-speakers)
* [AutoEq](https://github.com/jaakkopasanen/AutoEq)

reorganization the core preset in `simSpeaker.txt` file and adding some very **budget** but still hearable (maybe, I don't sure for everyone)
earbuds `AutoEQ` preset for best equalizing original sound

## Prerequisite

**Device & Headphone**
* any Windows PC
* FiiO FF1 Earbuds  ~ $15
* NiceHCK vido Earbuds (aka YUANDAO `p😭q`)   ~ 1$

**Software**
* Equalizer APO [Homepage](https://equalizerapo.com/) [Sourceforge](https://sourceforge.net/projects/equalizerapo/)

## Step

1. installation of Equalizer APO

2. copy all files inside `config/` folder into `C:\Program Files\EqualizerAPO\config`

3. [**with caution**] find / purchase `legel` reverb DSP plugin (maybe some `.dll` or other stuff) 
 added to the plugin folder `C:\Program Files\EqualizerAPO\VSTplugins` , please comply with law and regulation.

4. Setting any effective value like given below inside the reverb plugin

* PreDelay	5ms ( sound speed 340 m/s * 5 ms = 1.7m)
* Hold	10ms ( ~3.4m)
* Release	60ms ( (60-5-10)ms ~= 15.3m  ~= x3 fold of a 20 m^2 area)
* Diffuse	0%
* LoCut	20Hz
* HiCut	Off

## Configuration

listed below

|filename|description|
|--------|-----------|
|config.txt|EAPO main configuration file|
|config_ff1toHE1byraw.txt| Fiio FF1 frequency reponse(FR) equalization file |
|config_ff1autoeq.txt| Fiio FF1 frequency reponse(FR) equalization file |
|config_NiceHCKvidotoHE1raw.txt| Fiio FF1 frequency reponse(FR) equalization file |
|simSpeaker.txt| For Headphone-stereo speaker simulation |
|simSpeaker-01_模拟音箱串扰.txt| first one |
|simSpeaker-02_模拟房间反射.txt| second one |

## how these stuff comes from?

** example - make an FiiO FF1 to Sennheiser HE 1 (FR) simulation **

all inside the `config_ff1toHE1byraw.txt` file, and same as `NiceHCK vido p😭q` ones


**tl;dr** direct minus Frequency Response Curve of HE1 within FF1 earpod

newFR = -origin FR + target FR

below we using the smoothed metrics of FR, thus not suitable for daily use, 
we could just simply use given sufficed `_graphicEQ` config in each device folder from `AutoEQ`.

### smooth curve, for demostration
first, fetch the `smooth column` FR metrics of HE1 from `AutoEQ` project, which should sit in project `.csv` file

next, use the same method to fetch the headphone/earpod metrics 

(for `NiceHCK vido`, you need fetch FR metrics from legacy commit of the `AutoEQ` project, as recent one delete this for some reasons)

available commit id is [commit:9e35da7b](https://github.com/jaakkopasanen/AutoEq/blob/9e35da7be52aa379f8931f005a79d37f6ebd0523/results/referenceaudioanalyzer/referenceaudioanalyzer_siec_harman_in-ear_2019v2/NiceHCK%20Vido)

seperately save the `frequence:reponse` table in two file, format in `txt` or `csv`

now open the `EqualizerAPO`, and create a new config. after that, adding a first `filter` `Graphics EQ` for equalization headphone,
click the `import` icon in the right-(bottom-left) of the `G-EQ` panel, choose the `txt` of the headphone you `saved` from metrics 

after that, remember to click the `invert reponse` icon, as we want to first hard-'equalize' headphone/earpod

next, add the second `Graphics EQ` filter , this time is just the target `FR` metrics

using a `Preamplification` filter and choose suitable `db` for calibrate gain level.

then save the config to a seperate name, and include into the main `config`

### graphicEQ simulation (current->target),

first, create `G-EQ` for `AutoEQ-equalized` headphone/earpod
click `edit text` icon left, then direct copy the text givin in headphone/earpod profile from `AutoEQ` project, such text ended with `_GraphicEQ.txt`


next, create another `G-EQ` for `target response`. we simply choose Sennheiser HE1 `GraphicEQ.txt` to do so
then don't forget click `invert reponse` icon to make FR equalization, to `setback` the `AutoEQ-equalized` effort(which is actually some version of `Harman Curve`) done on HE1, thus this combination would make our current device repond like `Sennheiser HE1`