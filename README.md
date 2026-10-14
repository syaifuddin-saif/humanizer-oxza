# humanizer-oxza

[![License: MIT](https://img.shields.io/badge/license-MIT-2f8f57?style=flat)](LICENSE) ![version](https://img.shields.io/badge/version-1.0.0-blue?style=flat) ![languages](https://img.shields.io/badge/languages-Indonesian%20%7C%20English-orange?style=flat)

An agent skill that rewrites or reviews AI-sounding prose in **Indonesian and English** so it reads like the author, **without changing what it says**. It has a dedicated **academic mode** for journal articles, theses, dissertations, reviewer responses and grant proposals, where hedging, passive voice in Methods, numbers and citations must survive the edit.

It works in Claude Code, Claude.ai / Claude Desktop, and any other agent that supports `SKILL.md` skills.

> humanizer-oxza edits for human readers. It is **not** a tool for evading AI detectors or AI-use disclosure policies, and detectors may still flag its output (see [Limitations](#limitations)).

## Why another humanizer?

Most humanizer skills are English-only, built for blogs and marketing copy, and apply rules that damage academic writing. Some ban all passive voice, all adverbs, or every hedge such as "most" or "may". humanizer-oxza merges the strongest existing skills and corrects them with published research:

- **Bilingual.** Every pattern has an Indonesian equivalent, plus a section of nine Indonesian-specific tells (formulaic openers like *Di era digital saat ini*, *tidak hanya... tetapi juga*, bureaucratic phrasing like *dalam rangka*, register mixing, translation calques).
- **Safe for scholarship.** Evidence-tied hedging, passive voice in Methods, technical terms, equations, numbers and citation keys are protected. Verbs are matched to the strength of the evidence (*membuktikan* / *prove* becomes *menunjukkan* / *suggests* when the data only support that).
- **Research-backed.** The English word list comes from measured vocabulary shifts in more than 15 million PubMed abstracts, and the grammar rules follow corpus findings. For example, instruction-tuned models use agentless passive voice at roughly half the human rate, so banning passive voice pushes text *toward* the AI style rather than away from it.
- **Weighted, not absolute.** Each pattern is tagged **[KUAT]** (strong: edit on one sighting), **[SEDANG]** (medium: edit when repeated or combined) or **[LEMAH]** (weak: never edit alone), because any single tell can be a deliberate human choice.
- **No invented facts.** The skill never adds numbers, names, dates, sources or citations. When a claim lacks support, it flags `[perlu sitasi]` / `[perlu bukti]` for the author instead of making something up.

## Before and after

The examples below are illustrative. Numbers, citations and dataset names are placeholders, not real results.

### Indonesian (introduction paragraph)

**Before (AI draft):**
> Di era digital saat ini, Internet of Things (IoT) memainkan peran yang sangat krusial dalam berbagai sektor kehidupan. Protokol MQTT merupakan salah satu protokol komunikasi yang paling banyak digunakan dalam ekosistem IoT karena sifatnya yang ringan dan efisien. Namun demikian, broker MQTT tidak hanya rentan terhadap serangan, tetapi juga sering menjadi target utama serangan flooding — sebuah ancaman yang dapat melumpuhkan layanan secara keseluruhan. Lebih lanjut, berbagai penelitian menunjukkan bahwa metode deteksi konvensional belum mampu memberikan solusi yang komprehensif [3]–[5]. Oleh karena itu, penelitian ini hadir sebagai upaya untuk menggali secara mendalam pemanfaatan machine learning dalam rangka mendeteksi serangan flooding pada broker MQTT, dengan menguji algoritma Random Forest, XGBoost, dan LSTM pada dataset MQTTset [12]. Hasil penelitian ini diharapkan dapat memberikan kontribusi yang signifikan bagi perkembangan keamanan IoT di masa depan.

**After humanizer-oxza:**
> Protokol MQTT banyak dipakai pada perangkat IoT karena ringan dan efisien. Broker MQTT sering menjadi sasaran serangan flooding, yang dapat melumpuhkan seluruh layanan. Metode deteksi konvensional belum mampu menangani serangan ini secara memadai [3]–[5]. Penelitian ini menguji Random Forest, XGBoost, dan LSTM untuk mendeteksi serangan flooding pada broker MQTT dengan dataset MQTTset [12].

Removed: formulaic opener (G1), AI vocabulary *krusial, ekosistem, komprehensif, menggali* (§14), *tidak hanya... tetapi juga* (G2), em dash (§8), stacked connectors (G4), *dalam rangka* (G3), and an empty closing promise (G5, H2). Kept unchanged: citations [3]–[5] and [12], all three algorithms, and the dataset name.

### English (abstract)

**Before (AI draft):**
> In today's rapidly evolving digital landscape, ensuring the security of IoT ecosystems has become more crucial than ever. This study delves into the intricate challenge of detecting flooding attacks on MQTT brokers, leveraging a comprehensive machine learning framework. Not only does our approach achieve remarkable performance, but it also showcases robust generalization across diverse scenarios. Extensive experiments demonstrate that the proposed model attains an accuracy of 98.7% and an F1-score of 0.97, significantly outperforming existing methods. These findings underscore the pivotal role of intelligent traffic analysis and pave the way for more resilient IoT infrastructures.

**After humanizer-oxza:**
> Flooding attacks can disrupt an MQTT broker and the IoT services that depend on it. This study applies machine learning to detect such attacks from broker traffic. On the test set, the proposed model reached 98.7% accuracy and an F1-score of 0.97, higher than the existing methods it was compared with [name the baselines]. [Evidence needed: which scenarios support the claim of generalization?]

"Significantly" was dropped because no statistical test is reported, "demonstrate" was downgraded, and the unsupported generalization claim was flagged for the author rather than silently deleted. The bracketed notes are for the author and should be resolved before submission.

More examples: [`examples/before-after.md`](examples/before-after.md).

## Installation

### Claude Code (plugin)

```text
/plugin marketplace add syaifuddin-saif/humanizer-oxza
/plugin install humanizer-oxza@humanizer-oxza
```

### Skills CLI (Claude Code, Codex, Gemini CLI, Cursor and others)

```bash
npx skills add syaifuddin-saif/humanizer-oxza --global --agent claude-code
npx skills add syaifuddin-saif/humanizer-oxza --global --agent '*'   # every supported agent
```

Leave off `--global` to install for the current project only.

### Claude.ai and Claude Desktop

Download this repository as a ZIP (**Code → Download ZIP**) and upload it as a skill in Settings.

### Manual

Copy `SKILL.md` into your agent's skills folder, for example `~/.claude/skills/humanizer-oxza/SKILL.md`.

## Usage

Call the skill by name or ask in plain language, in either language:

```text
/humanizer-oxza
[paste your text]
```

```text
Tolong humanize paragraf pendahuluan ini, mode akademik: [teks]
```

```text
Review this abstract for AI tells, do not rewrite it: [text]
```

```text
Humanize the prose in drafts/chapter2.md and keep all citations unchanged.
```

For the best results, give the skill a sample of your own published writing. It will match your sentence length, hedging habits, transitions and terminology, and your sample overrides its default rules.

### Modes

| Mode | Use for | What changes |
|---|---|---|
| Academic | Journal articles, theses, abstracts, reviewer responses | Section H applies: hedging, passive voice in Methods, numbers and citations are protected; claim strength is matched to evidence |
| Proposal | Research, grant and community-service proposals | Ambitious vision language is allowed but every claim must have a feasibility anchor; reviewer-facing labels are allowed |
| Formal | Reports, terms of reference, official letters, SOPs | Bureaucratic conventions are kept where required |
| Casual | Blogs, popular articles, social media | The writer's opinions, humor and asides are kept |
| Reply | Email, comments, threads | The decision or answer comes first; context the reader already has is cut |
| Review only | "Check whether this sounds like AI" | No rewrite; returns findings with quote, pattern number, weight and suggested fix |

### What you get back

By default the final text and a short change report: patterns removed by type, claims softened or given evidence pointers, and confirmation that numbers, equations and citations are unchanged. In file mode only prose is edited; code, commands, paths, YAML, data tables, equations, citation keys and URLs stay as they are.

## What the skill checks

| Group | Patterns |
|---|---|
| A. Staging instead of stating | §1 Not X but Y · §2 one-line closers and dramatic fragments · §3 sayings that sound deep · §4 staged run-up · §5 arguing with no one |
| B. Rhythm by rule | §6 forced triads · §7 repeated openings · §8 dashes as universal connector · §9 stacked qualifiers · §10 hyphenated pairs · §11 missing actors and fake agency · §12 clause-stacked sentences · §13 choppy short-sentence runs |
| C. Inflation and borrowed authority | §14 AI vocabulary (measured English list, Indonesian practitioner list) · §15 inflated significance · §16 vague connection · §17 shallow -ing / -kan riders · §18 sales language · §19 borrowed authority · §20 avoiding *is / adalah* |
| D. Formatting by rule | §21 decorative bold and labeled lists · §22 decorative headings · §23 curly quotes |
| E. Chat and draft leftovers | §24 chatbot residue · §25 knowledge-limit disclaimers · §26 heading repeated in first sentence · §27 writing about the document |
| F. Wrong reader | §28 re-explaining what the reader knows, including reviewer responses |
| G. Indonesian-specific | G1 formulaic openers · G2 *tidak hanya... tetapi juga* · G3 bureaucratic phrasing and vague *hal ini* · G4 stacked connectors · G5 formulaic closers · G6 register mixing · G7 unnecessary English loanwords and calques · G8 synonym cycling · G9 empty intensifiers |
| H. Academic and proposal mode | H1 what to preserve · H2 eleven academic tells · H3 claim-evidence discipline · H4 voice and venue matching · H5 proposal mode |

## How it compares

| | stop-slop | humanizer (blader) | **humanizer-oxza** |
|---|---|---|---|
| Languages | English | English | **Indonesian + English** |
| Passive voice | Banned | Weak alone | Weak alone; protected in Methods and for Indonesian *di-* passives |
| Academic hedging | Removed | Only stacked hedges trimmed | **Required when evidence-tied** |
| Claim vs evidence check | No | No | **Yes**, with author flags |
| Numbers and citations | Not protected | Protected | Protected, citation count checked |
| Dashes | Banned, but its own examples use them | Banned unless in the writer's sample | Same as blader, plus EYD-valid number ranges (2020–2024) kept |
| Vocabulary source | Personal list | Practitioner list | **Corpus data** + practitioner list |
| Modes | 1 | Pasted text, file, embedded | + review-only, academic, proposal, reply |
| Ethics statement | None | Detectors not a goal | Explicit: not for detector evasion; disclosure policies still apply |

## Limitations

- **Detectors will often still flag the output.** In a quick test, the English "after" example above was still flagged as 100% AI by a commercial detector. That is expected. Statistical detectors read the predictability of word choices, which surface edits do not change, and in Russell et al. (2025) experts who use LLMs frequently still identified "humanized" text. Text reads as yours when it starts from your own draft and your own details.
- **The Indonesian patterns are practitioner heuristics.** No corpus study comparable to Kobak et al. exists yet for Indonesian, so section G is weighted conservatively. Contributions with data are welcome.
- **Detectors are not ground truth.** Liang et al. (2023) found that 61.3% of human-written TOEFL essays were flagged as AI-generated. Do not use this skill, or any detector, to accuse someone of using AI.
- **Disclose AI assistance** according to your journal's, funder's or university's policy. Editing a text with this skill does not change that obligation.
- The skill has not yet been evaluated with a blind human study. A small benchmark (pattern density, fact preservation, blind reader judgments) is planned.

## Research basis

- Wikipedia: [Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing), WikiProject AI Cleanup.
- Kobak, D., González-Márquez, R., Horvát, E.-Á., & Lause, J. (2025). Delving into LLM-assisted writing in biomedical publications through excess vocabulary. *Science Advances*, 11(27), eadt3813. [Data](https://github.com/berenslab/llm-excess-vocab).
- Reinhart, A., et al. (2025). Do LLMs write like humans? Variation in grammatical and rhetorical styles. [arXiv:2410.16107](https://arxiv.org/abs/2410.16107).
- Russell, J., Karpinska, M., & Iyyer, M. (2025). People who frequently use ChatGPT for writing tasks are accurate and robust detectors of AI-generated text. [ACL 2025](https://aclanthology.org/2025.acl-long.267).
- Juzek, T. S., & Ward, Z. B. (2025). Why does ChatGPT "delve" so much? [COLING 2025](https://arxiv.org/abs/2412.11385).
- Liang, W., et al. (2025). Quantifying large language model usage in scientific papers. *Nature Human Behaviour*. [doi:10.1038/s41562-025-02273-8](https://doi.org/10.1038/s41562-025-02273-8).
- Liang, W., et al. (2023). GPT detectors are biased against non-native English writers. *Patterns*. [arXiv:2304.02819](https://arxiv.org/abs/2304.02819).
- Herbold, S., et al. (2023). A large-scale comparison of human-written versus ChatGPT-generated essays. *Scientific Reports*, 13. [doi:10.1038/s41598-023-45644-9](https://doi.org/10.1038/s41598-023-45644-9).

## Credits

humanizer-oxza is a derivative work of these MIT-licensed projects. Their copyright notices are kept in [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md).

| Project | What was adapted |
|---|---|
| [blader/humanizer](https://github.com/blader/humanizer) v3.1 | Workflow, pattern weighting, patterns §1–§28, "when not to act" (translated and adapted) |
| [AIScientists-Dev/academic-humanizer](https://github.com/AIScientists-Dev/academic-humanizer) v0.3.3 | Academic and proposal mode (H1–H5) |
| [ariosenodev/humanizer-bahasa-indonesia](https://github.com/ariosenodev/humanizer-bahasa-indonesia) | Indonesian pattern catalog (G section), with new examples that do not invent facts |
| [williamkho77/claude-humanizer-id](https://github.com/williamkho77/claude-humanizer-id) | Indonesian patterns on *merupakan*, register mixing and loanwords |
| [hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop) | Fake agency (§11), meta-commentary (§27), staged openers (§4) |

## Contributing

Issues and pull requests are welcome, especially:

- Indonesian AI-writing patterns backed by examples or corpus counts.
- Cases where the skill over-corrects legitimate academic or official Indonesian writing.
- Before/after pairs from real (non-confidential) drafts.

Please keep examples free of invented facts. An "after" text may only contain information that is in the "before" text.

## Author

**Syaifuddin**, Informatics Department, Universitas Muhammadiyah Malang, Indonesia. GitHub: [@syaifuddin-saif](https://github.com/syaifuddin-saif).

## License

[MIT](LICENSE). See [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) for the licenses of the adapted works.
