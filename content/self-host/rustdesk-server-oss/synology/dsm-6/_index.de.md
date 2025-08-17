---
title: Synology DSM 6
weight: 22
---

> Ein alternatives aktuelles Tutorial von Dritten ist [hier](https://mariushosting.com/how-to-install-rustdesk-on-your-synology-nas/).

Dieses Tutorial basiert auf den neuesten DSM v6 und v7.

{{% notice note %}}
Nach dem DSM 7.2 Update wurde Docker auf den neuen "Container Manager" aktualisiert, bitte prüfen Sie [diesen Artikel](/docs/en/self-host/rustdesk-server-oss/synology/dsm-7) stattdessen.
{{% /notice %}}

## Docker installieren

| Paketzentrum öffnen | Docker installieren |
| --- | --- |
| ![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-6/images/package-manager.png) | ![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-6/images/docker.png) |

## RustDesk Server installieren

| Suchen Sie rustdesk-server in Docker's Registry und installieren Sie per Doppelklick | Installiertes rustdesk-server Image, doppelklicken Sie um rustdesk-server Container zu erstellen |
| --- | --- |
| ![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-6/images/pull-rustdesk-server.png) | ![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-6/images/rustdesk-server-installed.png) |

## hbbs Container erstellen

Wie oben erwähnt, doppelklicken Sie auf das rustdesk-server Image um einen neuen Container zu erstellen, setzen Sie den Namen auf `hbbs`.
![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-6/images/hbbs.png)

Klicken Sie auf die obigen `Erweiterte Einstellungen`.

- Aktivieren Sie `Automatischen Neustart aktivieren`.
![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-6/images/auto-restart.png)

- Aktivieren Sie `Dasselbe Netzwerk wie Docker Host verwenden`. Für mehr über Host-Netz, bitte [prüfen Sie](https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/docker/#net-host).
![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-6/images/host-net.png)

- Mounten Sie ein Host-Verzeichnis (z.B. `/home/rustdesk/`) zu `/root`, hbbs wird einige Dateien (Datenbank und `key` Dateien) in diesem Verzeichnis generieren, die über Neustarts hinweg bestehen bleiben müssen.

| Mount | Dateien generiert im Host-Verzeichnis |
| --- | --- |
| ![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-6/images/mount.png) | ![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-6/images/mounted-dir.png) |

- Befehl setzen
{{% notice note %}}
Synology's OS ist Debian-basiert, daher funktioniert Host-Netz (--net=host) gut, wir müssen keine Ports mit `-p` Option mappen.

{{% /notice %}}

![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-6/images/hbbs-cmd.png?v3)

- Fertig

## hbbr Container erstellen

Bitte wiederholen Sie die obigen `hbbs` Schritte, aber benennen Sie den Container `hbbr` und der Befehl (für Befehl setzen Schritt) sollte `hbbr` sein.

![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-6/images/hbbr-config.png)

## hbbr/hbbs Container

![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-6/images/containers.png)

| Doppelklicken Sie auf Container und prüfen Sie das Log | Doppelt bestätigen Sie hbbs/hbbr verwenden Host-Netzwerk |
| --- | --- |
| ![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-6/images/log.png) | ![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-6/images/network-types.png) |

## Ihren Schlüssel abrufen

Navigieren Sie zu dem zuvor eingerichteten Ordner mit File Station, laden Sie `id_ed25519.pub` herunter und öffnen Sie es mit einem Texteditor um Ihren Schlüssel zu sehen.