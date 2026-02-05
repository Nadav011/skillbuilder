# SINGULARITY FORGE

[![Release](https://img.shields.io/github/v/release/Nadav011/skillbuilder?display_name=tag)](https://github.com/Nadav011/skillbuilder/releases)
[![License](https://img.shields.io/github/license/Nadav011/skillbuilder)](https://github.com/Nadav011/skillbuilder/blob/main/LICENSE)
[![Stars](https://img.shields.io/github/stars/Nadav011/skillbuilder)](https://github.com/Nadav011/skillbuilder/stargazers)
[![Issues](https://img.shields.io/github/issues/Nadav011/skillbuilder)](https://github.com/Nadav011/skillbuilder/issues)

The Skill Architect Supreme for Claude Code. Builds and upgrades skills for Web and Mobile with a rigorous quality matrix, auto-heal, and research-first workflow.

## English

### Overview
SINGULARITY FORGE is a meta-skill for Claude Code that builds new skills and upgrades existing ones. It targets both Web (Next.js, React, TypeScript) and Mobile (Flutter/Dart 3.x, Riverpod) workflows, and enforces a strict multi-gate verification system to keep outputs consistent and production-grade.

### What Makes It Different
- 70-Gate quality verification across 8 clusters.
- Auto-heal for common failures like naming, filler, and RTL violations.
- Research-first workflow with Context7 and Memory MCP integration.
- Reflexion loop and uncertainty detection to improve reliability.
- Security-aware patterns aligned with OWASP LLM Top 10.
- Token ROI metrics and health dashboards.

### Key Capabilities
- Build new skills and upgrade legacy skills to the latest spec.
- Validate, heal, and compare skill versions.
- Platform-aware generation for Web or Flutter.
- Progressive disclosure via references for deep details.
- Automated metrics, health scoring, and self-evolution protocols.

### Commands

| Command | Purpose |
| --- | --- |
| `/forge [description]` | Build a new skill from a description. |
| `/forge --scaffold [name]` | Create a skill directory structure. |
| `/forge --validate [path]` | Validate a skill against the gate matrix. |
| `/forge --upgrade [path]` | Upgrade a skill to the latest spec. |
| `/forge --heal [path]` | Auto-heal common gate failures. |
| `/forge --metrics [path]` | Show Token ROI and quality metrics. |
| `/forge --health [path]` | Show a health dashboard score. |
| `/forge --compare [a] [b]` | Compare two skill versions. |
| `/forge --research [domain]` | Run research before building. |
| `/forge --test [path]` | Generate a test suite. |
| `/forge --benchmark [path]` | Benchmark against quality standards. |
| `/forge --wizard` | Guided interactive skill creation. |
| `/forge --compose [skills...]` | Compose multiple skills together. |
| `/forge --security [path]` | Security audit for vulnerabilities. |
| `/forge --flutter [name]` | Create a Flutter-focused skill. |

### Workflow (Core Phases)
1. Research relevant domain best practices.
2. Parse intent and choose skill type and complexity.
3. Select structure based on scope.
4. Generate `skill.yaml`, `SKILL.md`, references, and scripts.
5. Run the multi-gate verification matrix.
6. Auto-heal eligible failures.
7. Produce metrics and certification output.

### Upgrade Existing Skills
Upgrade a skill in place, validate it, then auto-heal any failed gates:

```bash
/forge --upgrade ~/.claude/skills/my-skill
/forge --validate ~/.claude/skills/my-skill
/forge --heal ~/.claude/skills/my-skill
```

### Installation
- Copy this folder to `~/.claude/skills/singularity-forge`.
- Ensure your Claude Code setup loads skills from `~/.claude/skills`.
- Use `/forge` commands in your Claude Code session.

### Validation Script
Run the validator against any skill:

```bash
./scripts/validate-skill.sh ~/.claude/skills/my-skill
```

### Repository Structure

```text
singularity-forge/
skill.yaml
SKILL.md
CHANGELOG.md
README.md
references/
assets/
scripts/
```

### Security and Quality
- Enforces RTL-first compliance for web and Flutter.
- Integrates OWASP LLM Top 10 patterns.
- Encourages verification, testing, and benchmark gates.

### Versioning
- Current version is defined in `skill.yaml`.
- See `CHANGELOG.md` for historical releases.

---

## עברית

### סקירה כללית
SINGULARITY FORGE הוא מיומנות-על ל־Claude Code שבונה מיומנויות חדשות ומשדרג מיומנויות קיימות. הוא תומך ב־Web (Next.js, React, TypeScript) וב־Mobile (Flutter/Dart 3.x, Riverpod), ומכיל מערכת אימות קפדנית עם שערי איכות לשמירה על תוצרים עקביים ומוכנים לפרודקשן.

### מה מייחד אותו
- אימות איכות עם 70 שערים ב־8 אשכולות.
- Auto-Heal לתקלות נפוצות כמו שמות, filler ו־RTL.
- תהליך מחקר לפני בנייה עם Context7 ו־Memory MCP.
- Reflexion Loop וזיהוי אי-ודאות לשיפור אמינות.
- דפוסי אבטחה בהשראת OWASP LLM Top 10.
- מדדי Token ROI ודשבורד בריאות.

### יכולות מרכזיות
- בניית מיומנויות חדשות ושדרוג מיומנויות קיימות לגרסה עדכנית.
- אימות, תיקון והשוואה בין גרסאות.
- יצירה מודעת־פלטפורמה ל־Web או Flutter.
- Progressive Disclosure דרך תיקיית references.
- מדדים אוטומטיים, ניקוד בריאות ו־Self-Evolution.

### פקודות

| פקודה | מטרה |
| --- | --- |
| `/forge [description]` | בניית מיומנות חדשה לפי תיאור. |
| `/forge --scaffold [name]` | יצירת מבנה תיקיות למיומנות. |
| `/forge --validate [path]` | אימות מול מטריצת השערים. |
| `/forge --upgrade [path]` | שדרוג מיומנות לגרסה האחרונה. |
| `/forge --heal [path]` | תיקון אוטומטי לתקלות נפוצות. |
| `/forge --metrics [path]` | הצגת Token ROI ומדדים. |
| `/forge --health [path]` | הצגת דשבורד בריאות. |
| `/forge --compare [a] [b]` | השוואת שתי גרסאות. |
| `/forge --research [domain]` | מחקר לפני בנייה. |
| `/forge --test [path]` | יצירת סט בדיקות. |
| `/forge --benchmark [path]` | השוואה לסטנדרטים. |
| `/forge --wizard` | מצב אשף אינטראקטיבי. |
| `/forge --compose [skills...]` | שילוב כמה מיומנויות. |
| `/forge --security [path]` | סקר אבטחה. |
| `/forge --flutter [name]` | יצירת מיומנות Flutter. |

### תהליך עבודה (שלבים עיקריים)
1. מחקר Best Practices בתחום הרלוונטי.
2. ניתוח כוונה ובחירת סוג/מורכבות.
3. בחירת מבנה לפי היקף.
4. יצירת `skill.yaml`, `SKILL.md`, references וסקריפטים.
5. אימות מול מטריצת השערים.
6. Auto-Heal לשערים שניתנים לתיקון.
7. מדדים והסמכת איכות.

### שדרוג מיומנות קיימת
שדרוג במקום, אימות, ואז תיקון אוטומטי לשערים שנכשלו:

```bash
/forge --upgrade ~/.claude/skills/my-skill
/forge --validate ~/.claude/skills/my-skill
/forge --heal ~/.claude/skills/my-skill
```

### התקנה
- העתקת התיקייה ל־`~/.claude/skills/singularity-forge`.
- ודא ש־Claude Code טוען מיומנויות מ־`~/.claude/skills`.
- השתמש בפקודות `/forge` בתוך סשן Claude Code.

### סקריפט אימות
הרץ את המאמת לכל מיומנות:

```bash
./scripts/validate-skill.sh ~/.claude/skills/my-skill
```

### מבנה ריפו

```text
singularity-forge/
skill.yaml
SKILL.md
CHANGELOG.md
README.md
references/
assets/
scripts/
```

### אבטחה ואיכות
- אכיפת RTL-first ל־Web ול־Flutter.
- שילוב דפוסים מ־OWASP LLM Top 10.
- עידוד אימות, בדיקות ו־Benchmark gates.

### גרסאות
- הגרסה הנוכחית מופיעה ב־`skill.yaml`.
- פירוט היסטוריה ב־`CHANGELOG.md`.
