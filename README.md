# QipIM for Android

[Русская версия](README.ru.md)

**QipIM for Android** is a revived **QIP Mobile**, an ICQ client for Android. The familiar QIP Mobile interface is kept,
but the old closed-source network core has been replaced by a new OSCAR protocol core written from scratch in Java.
The client connects to the modern OSCAR servers that keep ICQ alive: KICQ, MandarinIM, Nina.Chat, Logon.Chat,
Jimm.Online, CrossTalk, or any other OSCAR server.

> This project is not affiliated with QIP or its owners. "QIP" is a trademark of its owner and is mentioned only to
> say where the code comes from. The app is called **QipIM** and installs as `io.github.symnok.qipim`, so it can sit
> side by side with the original QIP.

> QipIM for Android is the sibling of [QipIM for Windows](https://github.com/Symnok/QipIM) (a QIP 2005-style client
> whose network core was ported from this app).

## Why QipIM was created

QIP Mobile was once one of the best ICQ clients for phones, but it has not been updated for a long time. On modern
devices it either does not install or does not work properly, and it lacks much of what is needed today.
QipIM fixes that.

### Android 6 and newer

The original QIP Mobile was made for old Android versions. QipIM reworks the app's system layer: running in the
background and keeping the connection alive, notifications, permissions, and saving profiles and open chats.
QipIM runs on **Android 6 and newer** (tested on Android 10 and newer).

### 64-bit devices

QIP Mobile's network core was a closed native library (`libqipcore.so`) built only for 32-bit processors. Many modern
phones run only 64-bit apps, and QIP Mobile does not start on them. In QipIM this library has been replaced entirely by
the new Java core; there is no native code in the app. So the same APK works on **both 32-bit and 64-bit** devices,
including those that do not support 32-bit apps at all.

### Symmetric encryption

A conversation with a contact can be encrypted with a shared passphrase (**Passphrase Encryption**): in the chat, tap
the paperclip at the top → *Passphrase Encryption* and enter the phrase your contact also uses. The server sees only
ciphertext.

- The key is derived from the passphrase with **Argon2id** (64 MiB, 3 passes).
- Each message is encrypted with **ChaCha20-Poly1305** with a random nonce and sent as Cyrillic letters (Base32).
- The scheme is compatible with **JasmineIM** (Symnok's fork), **SimpleOKM** and **Komet**, and with QipIM for Windows.
- Encrypted messages are marked with 🔒.

Limitations: no forward secrecy, the passphrase is stored in the app's private storage, and metadata (who, when,
size) is visible to the server.

### Avatars

Avatars are loaded the standard ICQ way, as in QIP 2005 and QipIM for Windows: a contact announces the ID of their
picture, and if it has changed, QipIM downloads it through the server's avatar service (SSBI). Your own avatar is shown
as well. If a contact removes their avatar, it disappears in QipIM too. An HTTP avatar server (as in Jasmine IM) is used
only as a fallback when the server has no avatar service.

### File transfer

Files are transferred over the ICQ protocol (OFT2) through a file transfer proxy server (ARS), the same way as in
Jasmine IM: *Send file* and *File transfers* in the chat menu; received files are saved to `Downloads/QipIM`. This
needs a server with such a proxy: in the **MandarinIM** profile it is already set up (`ars.mandarin.im`); for other
servers it can be entered in the profile settings.

## Servers

When adding an account, choose one of the ready-made profiles; the address and settings are filled in automatically
and can be changed in the profile's network settings.

| Profile | Server | Port | SSL/TLS | Login | File transfer server (ARS) | Avatar server (HTTP, fallback) |
|---|---|---|---|---|---|---|
| KICQ | `kicq.ru` | 5190 | no | MD5 | — | `http://217.147.15.238` |
| MandarinIM | `oscar.mandarin.im` | 5190 | no | MD5 | `ars.mandarin.im:5191` | `http://217.147.15.238` |
| Nina.Chat | `login.oscar.nina.chat` | 5190 | no | XOR | — | — |
| Logon.Chat | `logon.chat` | 5190 | no | XOR | — | — |
| Jimm.Online | `icq.jimm.online` | 5193 | yes | MD5 | — | — |
| CrossTalk | `crosstalk.im` | 5190 | no | MD5 | — | — |
| Generic OSCAR | any (enter it yourself) | 5190 | optional | MD5 | optional | optional |

On all servers, avatars are first loaded through the OSCAR server's own avatar service (SSBI); the HTTP avatar server
is used only if there is no such service. The XOR login type is the old method that Nina.Chat and Logon.Chat require.

## What else QipIM for Android can do

- **Several accounts** at once, including the same number on different servers.
- **Ready-made server profiles**: KICQ, MandarinIM, Nina.Chat, Logon.Chat, Jimm.Online, CrossTalk, and "Generic OSCAR"
  for any other server.
- **SSL/TLS** per profile, with certificate checking (Jimm.Online uses TLS by default).
- **X-statuses**: a picture with a title and text, as in QIP; contacts' X-statuses are shown.
- **Emoji/Smiles**
- **Avatars**
- **Account backup** to a file and restore from it; with a passphrase the backup is encrypted and includes passwords.



> QipIM was developed with the help of Claude, Anthropic's AI assistant: the analysis of the original APK, the new
> OSCAR core and the changes for modern Android were made together with Claude.
