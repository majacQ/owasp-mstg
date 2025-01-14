---
title: MobSF
platform: generic
source: https://github.com/MobSF/Mobile-Security-Framework-MobSF
---

[MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF "MobSF") (Mobile Security Framework) is an automated, all-in-one mobile application pentesting framework capable of performing static and dynamic analysis. The easiest way of getting MobSF started is via Docker.

```bash
docker pull opensecurity/mobile-security-framework-mobsf
docker run -it -p 8000:8000 opensecurity/mobile-security-framework-mobsf:latest
```

Or install and start it locally on your host computer by running:

```bash
# Setup
git clone https://github.com/MobSF/Mobile-Security-Framework-MobSF.git
cd Mobile-Security-Framework-MobSF
./setup.sh # For Linux and Mac
setup.bat # For Windows

# Installation process
./run.sh # For Linux and Mac
run.bat # For Windows
```

Once you have MobSF up and running you can open it in your browser by navigating to <http://127.0.0.1:8000>. Simply drag the APK you want to analyze into the upload area and MobSF will start its job.
