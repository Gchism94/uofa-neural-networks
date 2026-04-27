# CLAUDE.md — conventions for this repository

This repository hosts supplemental course visualizations for **INFO 557: Neural Networks**
at the University of Arizona, College of Information Science, taught by Greg Chism. The live
site is at <https://gchism94.github.io/uofa-neural-networks/> *(placeholder — confirm URL
before publishing)*.

> **Attribution:** This repository is forked from Theodore P. Pavlic's
> [asu-bioinspired-ai-and-optimization](https://github.com/tpavlic/asu-bioinspired-ai-and-optimization).
> Some visualizations are adapted from that source under the MIT License.
> © 2026 Theodore P. Pavlic.

---

## Registering an existing visualization

When a user asks to add an existing demo to the index/README/CLAUDE.md, **always also audit
the demo's HTML file itself** before finishing:

1. Check that `<head>` has a `<meta name="description">`, the full OG block, and the Twitter/X
   card block. If any are missing, add them (use the preview image dimensions from the actual
   file; aspect ratio should be close to 2:1 for Twitter).
2. Check that the bottom of `<body>` has the standard back-link `<footer>` and the
   iframe-hiding `<script>`. If missing, add them.

Do this proactively — the user should not have to ask separately.

---

## Adding a new visualization — full checklist

Each visualization lives in its own subdirectory:

```bash
my_demo/
  my_demo.html          # self-contained page (no build step)
  my_demo-preview.png   # preview image for OG/Twitter cards
```

### 1. `<head>` metadata in `my_demo.html`

Every demo page must have a proper HTML5 document structure (`<!DOCTYPE html>`, `<html lang="en">`,
`<head>`, `<body>`) — do not leave the file as a bare fragment.

Inside `<head>`, include all of the following, filling in the actual values:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Demo Title — interactive explainer</title>
<meta name="description" content="One or two sentences describing the demo.">

<!-- Open Graph (Facebook, LinkedIn, Slack, iMessage, etc.) -->
<meta property="og:type" content="website">
<meta property="og:title" content="Demo Title — interactive explainer">
<meta property="og:description" content="One or two sentences describing the demo.">
<meta property="og:image" content="https://gchism94.github.io/uofa-neural-networks/my_demo/my_demo-preview.png">
<meta property="og:image:width" content="ACTUAL_WIDTH">
<meta property="og:image:height" content="ACTUAL_HEIGHT">
<meta property="og:url" content="https://gchism94.github.io/uofa-neural-networks/my_demo/my_demo.html">
<meta property="fb:app_id" content="2385695445236853">

<!-- Twitter/X card -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="Demo Title — interactive explainer">
<meta name="twitter:description" content="One or two sentences describing the demo.">
<meta name="twitter:image" content="https://gchism94.github.io/uofa-neural-networks/my_demo/my_demo-preview.png">
</head>
```

**Twitter/X image requirements** (stricter than other platforms):

- Aspect ratio must be close to **2:1** (e.g. 1200×600, 2400×1200). Twitter silently drops
  images that deviate significantly, even if Facebook/Mastodon/Bluesky render them fine.
- File size must be **under 5 MB**.
- If the natural preview image is the wrong ratio, create a cropped/padded version for
  `twitter:image` while leaving `og:image` pointing at the full-resolution original.

### 2. Footer with back-link and iframe-hiding script

At the very bottom of `<body>`, before `</body>`, add:

```html
<footer id="course-nav-footer" style="margin-top:0;padding:0.75rem 0 0 1rem;border-top:1px solid #E0DAC0;font-size:0.8rem;color:#78786A;text-align:left;">
  <a href="../" style="color:#2e7d32;text-decoration:none;" onmouseover="this.style.textDecoration='underline'" onmouseout="this.style.textDecoration='none'">&larr; All course visualizations</a>
</footer>
<script>
if (window.self !== window.top) { var f = document.getElementById('course-nav-footer'); if (f) { f.style.display = 'none'; } }
</script>
```

The `<script>` hides the footer when the page is embedded in a Canvas LMS iframe. Use
`getElementById('course-nav-footer')` rather than `querySelector('footer')` — some demos
have their own internal `<footer>` elements, and `querySelector` would match the first one
it finds instead of the back-link footer.

**Watch for body padding:** if the demo's `body` CSS has no `padding-bottom`, the footer will
sit flush against the viewport edge. Add `padding-bottom` to the body or `margin-bottom` to
the footer if needed.

### 3. Entry in `index.html`

Add a `<li>` inside the correct `<section class="demo-section">` in `index.html`. Sections
are delimited by HTML comments; each has a placeholder comment marking where to insert:

```html
<li>
  <a class="demo-row" href="my_demo/my_demo.html">
    <img class="demo-thumb"
         src="my_demo/my_demo-preview.png"
         alt="My Demo preview"
         width="120" height="90">
    <div class="demo-text">
      <h3>Demo Title — interactive explainer</h3>
      <p>One sentence description that conveys what the demo shows and why it matters for the course.</p>
    </div>
  </a>
</li>
```

Current sections and their placeholder comments:

| Section heading | Placeholder comment |
| --- | --- |
| Probabilistic Foundations | `<!-- Add more probabilistic foundations demos here -->` |
| Neural Networks | `<!-- Add more neural network demos here -->` |

To add a **new section**, copy the structure of an existing `<section class="demo-section">`
block and add a corresponding nav link in the `<nav>` at the top of the page.

### 4. Entry in `README.md`

Add a row to the appropriate table under `## Contents`:

```markdown
| [`my_demo/`](my_demo/) | Brief description matching the index entry |
```

---

## Site structure

- `index.html` — the root landing page; self-contained HTML (no Jekyll/build step)
- `README.md` — GitHub repo landing page; mirrors the index structure for repo visitors
- Each demo is a **self-contained, single-file HTML page** with all CSS and JS inlined
- Preview images live alongside their HTML file in the same subdirectory
- The site is deployed via **GitHub Pages** directly from the `main` branch (no build step)

## Current sections and demos

### Probabilistic Foundations

- `softmax/softmax_temperature_explorer.html`
- `maxent/maxent_demo.html`
- `boltzmann_maxent/boltzmann_maxent_random_exchange.html`

### Neural Networks

- `single_layer_perceptron/slp_explainer.html`
- `radial_basis_function_nn/rbfnn_explorer.html`
- `multi_layer_perceptron/mlp_explorer.html`
- `optimal_foraging_theory/mvt_explorer.html`
- `spiking_neural_networks/snn_explorer.html`
- `memristors/memristor_stdp_array.html`
- `hebbian_learning/hebbian_competitive_clustering.html`
- `recurrent_neural_networks/rnn_explorer.html`
- `reservoir_computing/esn_explorer.html`
- `unsupervised_learning/autoencoder_explorer.html`
- `transformers/transformer_explorer.html`
- `transformers/toward_multimodal_AI.html`
- `activation_functions/af_explorer.html`
- `gradient_descent/gd_explorer.html`
- `backpropagation/bp_explorer.html`
- `regularization/l1l2_explorer.html`
- `dropout/dropout_explorer.html`
- `early_stopping/es_explorer.html`
- `optimizers/optimizer_explorer.html`
- `batch_norm/bn_explorer.html`

---

## Course unit structure

Use this to decide which section a new demo belongs in. Section titles deliberately match the
course unit vocabulary so students can orient themselves.

| Course unit | Topics | Index section |
| --- | --- | --- |
| Unit 1 | Activation functions, feedforward networks, XOR problem | Neural Networks |
| Unit 2 | Cost functions, gradient descent, backpropagation, computational graphs | Neural Networks |
| Unit 3 | Regularization (L1/L2, dropout, early stopping, batch normalization) | Neural Networks |
| Unit 4 | Optimization algorithms (SGD, momentum, Adam, RMSProp, AdaGrad) | Neural Networks; Probabilistic Foundations |
| Unit 5 | Convolutional networks (convolution, pooling, translation invariance) | Neural Networks |
| Unit 6 | Recurrent networks (RNNs, BPTT, LSTMs, GRUs, bidirectional RNNs) | Neural Networks |
| Unit 7 | Transformers and attention mechanisms | Neural Networks |
| Unit 8 | Model interpretability (LIME, gradient attribution) | Neural Networks |

**Section title rationale:**

- *"Neural Networks"* — the primary section; covers all eight course units: feedforward
  architectures, training dynamics, regularization, optimization, CNNs, RNNs, Transformers,
  and interpretability.
- *"Probabilistic Foundations"* — covers the mathematical underpinnings shared across units:
  the Gibbs/softmax distribution, Maximum Entropy, and the Boltzmann distribution. These
  motivate temperature scaling in inference, energy-based models, and the information-theoretic
  view of neural network training.

## Shared conventions

- **Accent color:** `#2e7d32` (dark green) — used in links and section headings
- **Copyright:** © 2026 Greg Chism, MIT License (`LICENSE` file at repo root)
  — © 2026 Theodore P. Pavlic (adapted visualizations, MIT License)
- **fb:app_id:** `2385695445236853` — include in all OG blocks
- **GitHub Pages base URL:** `https://gchism94.github.io/uofa-neural-networks/` *(placeholder)*
- **Attribution note:** Some visualizations adapted from Theodore P. Pavlic, ASU (MIT License)