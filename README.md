## Scheme Identification

We tested all available watermark decoders, including:

- **imwatermark:** `dwtDct`, `dwtDctSvd`, `RivaGan`
- **TrustMark:** `Q`, `P`, `C`
- **blind_watermark`

Each decoder was evaluated on every `WM_1`–`WM_8` source set using several payload lengths. For each candidate, all 25 images were decoded, and a majority vote was taken for every bit. The agreement between individual decodes and the majority message was used to identify the scheme. A decoder was considered a match when the agreement exceeded **85%**, while incorrect decoders typically achieved only **~50%** agreement.

Identified schemes:

| Scheme | Identified Method |
|--------|-------------------|
| WM_1 | imwatermark (dwtDct) |
| WM_2 | imwatermark (RivaGan) |
| WM_7 | TrustMark (Q) |
| WM_8 | TrustMark (P) |

All identified schemes achieved **>98%** decoding agreement.

---

## Decode-and-Re-encode Attack

For identified watermarking schemes, we:

1. Decoded all source images.
2. Recovered the watermark message using majority voting.
3. Re-embedded the recovered message into clean target images using the corresponding encoder.

Since the original encoder was reused, no additional embedding-strength tuning was required.

---

## Blind Fingerprinting

The remaining schemes (WM_3–WM_6) could not be identified using any available decoder. For these schemes, we estimated the watermark as a shared additive fingerprint across the 25 watermarked images following the *simple averaging* idea of Yang *et al.* (NeurIPS 2024).

The fingerprint was estimated by:

- Computing a spatial high-pass residual (image − blurred image).
- Computing a frequency-domain band-pass residual using FFT.
- Aggregating residuals across all images using the **median**.
- Applying **MAD-based** normalization for robustness.
- Clipping the estimated fingerprint before adding it to clean target images.

This produced a robust watermark estimate while maintaining good visual quality.
