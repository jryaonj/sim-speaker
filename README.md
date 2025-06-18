# Headphone Stereo Speaker Simulation

Fetched from `xinleio/Headphones-simulate-stereo-speakers` virtual stereo settings and using 
`jaakkopasanen/AutoEq` headphone/earbuds equalization project

* [Headphones-simulate-stereo-speakers](https://github.com/xinleio/Headphones-simulate-stereo-speakers)
* [AutoEq](https://github.com/jaakkopasanen/AutoEq)
* **Use [AutoEq.app](https://autoeq.app) for online results** (use with caution - see the [Recommended Process](#recommended-process) section below for the recommended approach)

This project reorganizes the core presets into `simSpeaker.txt` and adds AutoEQ configurations for various earbuds to improve their sound quality.

## Prerequisites

**Hardware:**
- Any Windows PC
- Supported earbuds/headphones:
  - FiiO FF1 Earbuds (~$15)
  - NiceHCK Vido Earbuds (aka YUANDAO WUJI `p😭q`) (~$1)
  - 1MORE E1008 Earbuds (~$35)
  - Pioneer SE-A1000 Headphone (~$35)

**Software:**
- [Equalizer APO](https://equalizerapo.com/)

## Setup Instructions

1. Install Equalizer APO
2. Copy all files from the `config/` folder to `C:\Program Files\EqualizerAPO\config`

## Configuration Files

| Filename                         | Description |
|----------------------------------|-------------|
| config_full.txt                  | Complete configuration including speaker simulation and equalization |
| config.txt                       | Basic configuration without advanced features |
| config_e1008.txt                 | 1More E1008 equalization configuration |
| config_ff1byraw.txt              | FiiO FF1 equalization using manual level flat + Harman target |
| config_fiioff1autoeq.txt         | FiiO FF1 equalization using AutoEQ preset |
| config_NiceHCKvido.txt           | NiceHCK Vido to Harman in-ear 2019 with bass |
| config_NiceHCKvidotoHE1byraw.txt | NiceHCK Vido to Sennheiser HE1 equalization |
| config_sea1000.txt               | Pioneer SEA1000 equalization configuration |
| simSpeaker.txt                   | Main headphone-to-stereo speaker simulation |
| simSpeaker-01_模拟音箱串扰.txt   | Speaker crosstalk simulation (Essential) |
| simSpeaker-02_模拟房间反射.txt   | Room reflection simulation (Optional - see notes below) |

**Important Notes on Room Reflection Effects:**
- **Avoid `simSpeaker-02` for critical listening:** Room reflections typically degrade sound quality by adding unnatural reverberation. Acoustic research shows minimal reflections provide the cleanest reproduction.
- **Manual Reverb Configuration (Optional - Not Recommended):**
  Found any VST plugin which provides high-quality reverb processing on room simulation. For small room simulation (if desired), configure with these settings:

  | Parameter  | Value    | Physical Equivalent      | Rationale                           |
  |------------|----------|--------------------------|-------------------------------------|
  | PreDelay   | 5ms      | ~1.7m distance           | 340 m/s × 0.005s = 1.7m wave travel|
  | Hold       | 10ms     | ~3.4m distance           | Corresponds to small room dimensions|
  | Release    | 60ms     | ~15.3m distance          | Natural decay for small spaces     |
  | Diffuse    | 0%       | N/A                      | Prevents muddying of direct sound  |
  | LoCut      | 20Hz     | Sub-bass frequencies     | Removes unnecessary rumble         |
  | HiCut      | Off      | Full high frequencies    | Preserves audio clarity            |

## Creating Custom Configurations

### Recommended Process

1. **Obtain frequency response data** for your headphones:
   - Find measurements online (graphs or data). Ensure the data is gained by GRAS equipment/method measured, as only Harman response curve can be directly compensated (both use HTRF processes). For other measurement systems, you'll need to find a harman->target equipment transition function/compensation.
   - Use [WebPlotDigitizer](https://automeris.io/wpd/) to extract data points from graphs
   
2. **Process the data**:
   - Convert to CSV format
   - Format for Equalizer APO using GraphicEQ syntax
   - Example conversion (using GNU sed):
     ```bash
     sed 's/\n/; /g' input.csv | sed 's/,//g' > output.txt
     ```

3. **Create Equalizer APO configuration**:
   - Create new configuration in Equalizer APO
   - Add GraphicEQ filter with your headphone's inverted response
   - Add second GraphicEQ filter with target frequency response (Harman curve)
   - Adjust preamplification to prevent clipping

### Example Configurations

- **FiiO FF1 to Harman In-Ear 2019** (`config_ff1byraw.txt`):
  ```text
  newFR = -original_FR + harman_target_FR
  ```

- **NiceHCK Vido to Harman In-Ear 2019** (`config_NiceHCKvido.txt`):
  ```text
  newFR = -vido_FR + harman_target_FR
  ```

- **1more E1008 to Harman In-Ear 2019** (`config_e1008.txt`):
  ```text
  newFR = -e1008_FR + harman_target_FR
  ```

- **Pioneer SE-A1000 to Sennheiser HE1 Orpheus ** (`config_sea1000.txt`):
  ```text
  newFR = -a1000_compensated_FR + innerf_target_FR + HE1_FR
  ```

### Important Considerations

- Use raw frequency response measurements when available
- For pre-compensated data (e.g., InnerFidelity), apply appropriate compensation curves from `misc/`
- Always verify results with listening tests
- Target curves:
  - `misc/Harman in-ear 2019.csv`
  - `misc/Harman over-ear 2018.csv`

## Resources
The `misc/` directory contains:
- Raw frequency response data (`*.csv`)
- Compensation curves
- Reference images
- Additional tools and resources
