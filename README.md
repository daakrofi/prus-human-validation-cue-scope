# PRUS Human Validation Site — Cue-and-Scope Hybrid

This repository contains an alternative GitHub Pages website for post-level human validation of Pre-Release User Speculation (PRUS). It uses the same blinded 500-post sample as the main validation site, but applies a human-friendly codebook derived primarily from the V2 rubric used to label the synthetic video-game sentence dataset.

## Validation Unit

The unit presented to coders is a Steam topic-root post, represented by the post title and the captured post body/excerpt retained in the analysis corpus. This matches the unit at which sentence-level PRUS evidence is aggregated in the study. The website does not ask coders to judge isolated sentences.

A post is coded **PRUS** when its title, one sentence, or several connected passages (1) put forward a possible claim, explanation, prediction, interpretation, theory, or unresolved alternative about the game or product experience and (2) present that idea with reduced commitment. The rest of the post may contain factual, affective, or otherwise non-PRUS material. Coders select every applicable product topic domain for PRUS-positive posts.

The alternative deployment is intentionally separate from the main validation site. Its distinct sample version, browser-storage prefix, and private response namespace allow instruction effects to be compared without changing the live main codebook or overwriting its responses.

## Files

- `index.html`: validation interface
- `styles.css`: responsive site styling
- `app.js`: browser persistence, secure checkpoints, resume, and response export
- `config.js`: secure response endpoint and checkpoint interval
- `worker/`: Cloudflare Worker that validates submissions and writes them to the private GitHub response repository
- `data/sample_posts.json`: blinded 500-post validation sample used by the site

Model labels, class allocation, sampling strata, selection thresholds, randomization details, and diagnostic materials are retained outside the public application repository. The browser receives only the post information required to complete the exercise.

The captured body sometimes ends with an ellipsis because the source Steam discussion corpus retained a topic summary/excerpt rather than retrieving a later full thread page. The validation interface reproduces the same captured text that entered the sentence-level analysis, preserving measurement alignment.

## Local Use

```bash
python3 -m http.server 8787
```

Open `http://127.0.0.1:8787`.

## Persistence and Secure Collection

The site stores a browser copy of participant progress in `localStorage` and sends secure checkpoints to a Cloudflare Worker. The Worker validates each submission and commits the latest post-validation record to the private `daakrofi/prus-human-validation-responses` repository.

Post-level responses are stored under `responses/post-validation-synthetic-rubric-hybrid-v1/`. This keeps them separate from the main post-validation responses and from the superseded sentence-level exercise.

Participant email addresses are normalized and hashed before the GitHub path is created. Names, email addresses, and telephone numbers do not appear in filenames or commit messages. They remain inside the response JSON in the private repository.

The site creates a remote checkpoint every 25 completed posts, when the participant selects **Save and Exit**, and after all 500 posts have been completed. Browser-side checkpoint requests are serialized and coalesced. The Worker retries transient GitHub conflicts and prevents an older checkpoint from replacing a more advanced participant record.

## Response Export

At completion, coders can download CSV or JSON backups containing participant details, post metadata and displayed text, the human PRUS label, all selected product topic domains, and the answer timestamp. Model labels are joined only after collection through the private audit file.
