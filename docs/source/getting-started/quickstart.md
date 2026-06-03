# Quickstart

```{note}
This section is under development.
```

This guide walks through running GloGEM for a single glacier to verify your installation.

## 1. Set up your personal config

Follow the [Configuration](configuration.md) guide to create `~/.glogem/config.pro` with at minimum your output directory.

## 2. Select a single glacier

In `~/.glogem/config.pro`, set:

```idl
single_glacier = '01450'   ; Aletschgletscher (RGI ID)
calibrate = 'n'
time_resolution = 'monthly'
tran = [2000, 2020]
```

## 3. Run the model

Open IDL in the GloGEM repository directory and run:

```idl
.run glogem.pro
```

A log file will be written to `logs/` and results to the directory specified in `dirres`.

## 4. Check the output

```{note}
Output file formats are described in [Output Files](../running-glogem/output-files.md).
```
