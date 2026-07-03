# AI Memory & Content Guardrails

**This file acts as a permanent memory for all future AI interactions regarding this website and content generation. You MUST read and follow these rules every time.**

## 1. PyTorch Only (BANNED: Keras/TensorFlow)
- The user is mastering PyTorch. **Never generate code using Keras or TensorFlow.**
- All implementations must use `torch.nn`, `torch.utils.data.DataLoader`, and standard PyTorch training loops.

## 2. Data Storytelling & Dashboards (Senior AI Engineer Level)
- **Deep Dives Only:** Never generate surface-level "what is X" posts.
- **Dashboards:** Site posts must include visual dashboards (e.g., correlation heatmaps, loss curves, distribution graphs) generated from the actual dataset to show Senior-level Data Analyst skills. Use Google best practices.
- **Storytelling:** Explain the *business value* and *data story* of the KPIs, not just the raw code.
- **Real Datasets:** For any AI/ML topic, you MUST load and process a real dataset from `DATASETS/` (e.g., `water_potability.csv`). 

## 3. Branding & Terminology
- **Portfolio Projects, Not Labs:** Never use the terms "TP" (Travaux Pratiques), "Lab", or "University" in the public website or LinkedIn posts. Always frame them as "Portfolio Projects" or "Deep Dives".
- **Tone:** Professional, easily digestible, jargon-free, but highly technical under the hood. It must sound human, sleek, and capable of impressing HR and fellow engineers.

## 4. LinkedIn Carousel Design System
- **Template:** Always use the beautiful gradient design. This means including the `.circle` background element, `.swipe-hint` at the bottom right, and perfectly centered content.
- **Fonts:** DO NOT use Google Fonts. You must load the local font using:
  ```css
  @font-face {
      font-family: 'SF Pro Display';
      src: url('../../portfolio_site/Web Fonts/f80bfd00af2dfcccf50416a47dd960da.woff2') format('woff2');
  }
  ```
- **Editable Drafts:** All generation should be done via `scripts/build_content.py` using editable JSON/Markdown drafts so the user can tweak the words easily before final rendering.
