# Configuration

GloGEM separates shared model settings from personal, machine-specific settings.

## Shared settings: `settings.pro`

The file `procedures/initialise/settings.pro` contains all model settings committed to the repository. This file defines defaults for every parameter — time resolution, region selection, GCM choice, calibration options, output variables, and physical constants.

Do not add personal paths or experiment-specific values here. Changes to this file affect everyone working from the same repository.

## Personal overrides: `~/.glogem/config.pro`

Each user maintains a personal config file at `~/.glogem/config.pro`. This file lives in your home directory and is never touched by git. Any setting from `settings.pro` can be overridden here.

### One-time setup per machine

```bash
cd /path/to/GloGEM      # must be run from inside the repository
mkdir -p ~/.glogem
cp config.pro.example ~/.glogem/config.pro
```

Then open `~/.glogem/config.pro` and set at minimum your output directory:

```idl
dirres = '/path/to/your/output/directory'
```

The model will stop with a clear error message if `dirres` is not set.

### Typical personal overrides

```idl
; Output directory (required)
dirres = '/scratch/username/glogem_output/'

; Run a single glacier instead of a full region
single_glacier = '01450'   ; Aletschgletscher

; Select region
region_id_loop = [11, 11]  ; Central Europe

; Time resolution
time_resolution = 'daily'
```

For a full list of available settings, see the [Settings Reference](../running-glogem/settings-reference.md).
