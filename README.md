# Gunbound Characterdata.dat EDITOR
---

## Features

- **Decrypt** — Convert an encrypted `.dat` binary into raw plaintext data
- **Encrypt** — Convert raw data back into an encrypted `.dat` binary (iXFS checksum appended automatically)
- **Character Editor** — Edit per-mobile attributes directly from `characterdata.dat`
- **Compare Mode** — Load two files side-by-side and highlight value differences
- **Batch Edit** — Apply a ±delta to one attribute across all 19 mobiles at once

---

## Algorithm

| Parameter | Value |
|---|---|
| Algorithm | **LZHUF** (LZ77 + Adaptive Huffman Coding) |
| Author | H. Yoshizaki, 1988 |
| Buffer Size (N) | 4096 |
| Lookahead (F) | 60 |
| Threshold | 2 |
| Trailing Byte | iXFS checksum = `(0x4D + Σ plain bytes) & 0xFF` |

---

## Supported Files

| File | Known Length (uncompressed) |
|---|---|
| `characterdata.dat` | **6640 bytes** |
| `stage.dat` | **39936 bytes** |

---

## Usage

1. Download `gb_crypt_gui_v3_vscode.html`
2. Open it directly in any modern browser (Chrome / Firefox / Edge)
3. No installation or internet connection required

### Decrypt
1. Go to the **⬇ DECRYPT** tab
2. Drop or select an encrypted `.dat` file
3. Set the **Known Length** for your file type (default `6640` for `characterdata.dat`)
4. Click **DECRYPT FILE** → download the output

### Encrypt
1. Go to the **⬆ ENCRYPT** tab
2. Drop or select a decrypted (plaintext) `.dat` file
3. Click **ENCRYPT FILE** → download the output

### Character Editor
1. Go to the **✎ EDITOR** tab
2. Click **Open Decrypted** or **Open Encrypted** (auto-decrypts on load)
3. Select a mobile from the dropdown
4. Edit attribute values directly in the table
5. Use **Compare** to load a second file for side-by-side comparison
6. **Save Decrypted** → save as plaintext  
   **Save & Encrypt** → save directly in encrypted format

---

## Supported Mobiles (Character Editor)

Armour · Mage · Nak · Trico · Bigfoot · Bommer · Raon · Lighting · JD · A.Sate · ICE · Turtle · Grub · Aduka · J.Frog · Kalsiddon · Dragon/Kalsiddon · Knight · Phoenix

---

## Technical Notes

- A full roundtrip (decrypt → encrypt) produces a **byte-for-byte identical** file to the original
- The iXFS checksum is computed and appended automatically during encryption
- The editor reads per-mobile offsets from `characterdata.dat` using the following formula:
  ```
  base_offset = 0xD4C
  offset_step = 0x14C
  car_offset(idx) = base_offset - (11 - (idx + 1)) * offset_step
  ```
- All attribute values are stored as **unsigned 16-bit little-endian** (`uint16_le`), valid range: `0–65535`

---

## File

| File | Description |
|---|---|
| `gb_crypt_gui_v3_vscode.html` | Single-file web app — just open it in a browser |

---

## Credits

- LZHUF algorithm — H. Yoshizaki (1988)
- GunBound file format research — GunboundStageEditorS2 / iXFS
