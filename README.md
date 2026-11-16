# Nimbus downloads

Public binary releases for **Raspberry Pi OS Trixie 64-bit Lite**. Application source is maintained separately. The pipeline is being prepared; no installer has been published or qualified yet.

[Download releases](https://github.com/Droneforge-Inc/nimbus-releases/releases). Assets include the application archive, `nimbus-pi.json`, `nimbus-pi.sig`, `SHA256SUMS`, and build provenance. The archive carries Flutter, native helpers, CPU inference models, runtime libraries/notices, offline Trixie runtime packages, and an installer. GitHub's generated source archives contain this documentation, not the application.

Before extraction, verify `nimbus-pi.json` using the [release public key](pi-release-public.pem), then compare the archive's size and SHA-256 against the signed manifest:

```sh
openssl dgst -sha256 -verify pi-release-public.pem -signature nimbus-pi.sig nimbus-pi.json
```

After verification, extract into an empty directory and run:

```sh
sudo python3 installer/pi_install.py --user YOUR_KIOSK_USER
sudo systemctl start nimbus-kiosk.service
```

Use a stopped, disarmed controller. Existing installations can pass `--data-directory /existing/kiosk/working/directory` to preserve recordings and enrollment. Installation requires no compiler or internet connection. Kernel/camera drivers, display configuration and the Trixie OS baseline remain device provisioning responsibilities.

The installed kiosk runs as your regular user, starts after boot, and checks this repository every 15 minutes. It stages signed newer compatible releases alongside the running application. Settings → Application offers **Restart to update** after staging. You can keep using the current app; a staged update also activates on the next power-on. A failed or interrupted startup trial restores the previous application automatically.

CI validation is separate from camera, display and flight qualification. Pilot each release before fleet rollout.
