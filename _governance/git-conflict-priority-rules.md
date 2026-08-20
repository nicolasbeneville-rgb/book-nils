<!-- TYPE: AUDIT | CATEGORIE: GIT_CONFLICT_RESOLUTION | SYNC: NON | PROPAGATION: REFERENCE -->

# Git Conflict Priority Rules — Multi-PC Resolution

**Scope** : Handle file conflicts when pulling from remote after work by multiple PCs  
**Authority** : Single source of truth for conflict resolution strategy  
**Modification** : Update when new file categories need special handling  
**Reference** : Cited by `#bonjour` §1 (conflict detection) and conflict resolution scripts

---

## File Categories & Priority Rules

### 1️⃣ SYNC Files (`_governance/` source + distributed copies)

**Files** : 
- `_governance/core/copilot-instructions-commun.md`
- `_governance/overlay-gas/copilot-instructions-gas.md`
- `_governance/overlay-react/copilot-instructions-react.md`
- `_governance/agent-deployer-template.md`, `skill-check-account-template.md`, etc.
- `.github/copilot-instructions-commun.md` (copies in each project)
- `.github/copilot-instructions-gas.md` ou `.github/copilot-instructions-react.md` (copies selon signature)
- `docs/retro-modele.md` (copies in each project)

**Conflict Rule** : **REMOTE > LOCAL**

**Rationale** : SYNC files are governance masters — remote version is always authoritative. Local copies are overwritten by sync scripts.

**Resolution Command** :
```bash
git checkout --theirs <file>
git add <file>
# Message: "[SYNC] <file> — accepted remote version (sync source of truth)"
```

**User Action** : None required — automatic resolution

---

### 2️⃣ LOCAL Files (`.github/copilot-instructions.md` only)

**Files** :
- `.github/copilot-instructions.md` (project-specific)
- Any custom LOCAL rules per project

**Conflict Rule** : **LOCAL > REMOTE**

**Rationale** : LOCAL instructions are project-specific customizations. Each PC's LOCAL is authority for its own project context. If 2 PCs modified LOCAL differently, human arbitration needed.

**Resolution Command** :
```bash
git checkout --ours <file>
git add <file>
# Message: "[LOCAL] <file> — retained local version — requires human arbitration"
```

**User Action** : **MANDATORY** — Review conflict with team, decide which customization wins, update decision-log

---

### 3️⃣ AUDIT Files (Project-specific, non-synced)

**Files** :
- `docs/project/operating-rules.md`
- `docs/project/decision-log.md`
- `docs/project/retro-modele.md` (project-level, different from root copy)
- `docs/project/architecture-notes.md`

**Conflict Rule** : **LOCAL > REMOTE**

**Rationale** : AUDIT files are project-specific documentation. Each project's rules are local truth. If 2 PCs added different rules, both are kept (append, not replace).

**Resolution Command** :
```bash
# For retro/decision-log: manual merge (append missing lines from both versions)
git checkout --ours <file>
git add <file>
# Message: "[AUDIT] <file> — retained local version — verify merge completeness"
```

**User Action** : **MANDATORY for append-only** — Verify both versions merged, no content lost

---

### 4️⃣ Append-Only Files (`session-log.md`, decision logs)

**Files** :
- `_governance/session-log.md`
- `docs/project/decision-log.md` (if append-only)
- `.backups/` (manifests if tracked)

**Conflict Rule** : **MERGE (APPEND)**

**Rationale** : These files grow with each session/decision. Never overwrite — always append missing entries from both versions.

**Resolution Algorithm** :
```
1. Read local version (all lines)
2. Read remote version (all lines)
3. Find lines unique to remote (not in local)
4. Append remote-only lines to local
5. Sort by timestamp if applicable
6. Write merged result
7. git add <file>
```

**Resolution Example** :
```
# Conflict in session-log.md

# Local version:
| 2026-07-08 22:30 | PC-A | Audit360 | a3f9c12 | NON | CLEAN |
| 2026-07-09 08:00 | PC-A | Audit360 | b7e2a45 | OUI | CLEAN |

# Remote version (PC-B pushed):
| 2026-07-09 09:15 | PC-B | Audit360 | c9d4e67 | NON | CLEAN |

# Merged result (append PC-B's entry):
| 2026-07-08 22:30 | PC-A | Audit360 | a3f9c12 | NON | CLEAN |
| 2026-07-09 08:00 | PC-A | Audit360 | b7e2a45 | OUI | CLEAN |
| 2026-07-09 09:15 | PC-B | Audit360 | c9d4e67 | NON | CLEAN |
```

**User Action** : Verify no duplication, confirm timestamp ordering

---

### 5️⃣ Project Code Files (`.js`, `.html`, `.json` application code)

**Files** :
- `Code.js`, `ActionService.js`, etc. (GAS code)
- `Index.html`, `Admin.html` (UI code)
- `appsscript.json`, `package.json` (configs)

**Conflict Rule** : **HUMAN MEDIATION**

**Rationale** : Code conflicts require developer decision — no automatic rule.

**Resolution Command** :
```bash
# Don't auto-resolve — require developer merge
git status  # Shows unmerged path
# Developer manually edits file, removes conflict markers
git add <file>
# Message: "[CODE] <file> — manual merge completed — verify logic"
```

**User Action** : **MANDATORY** — Resolve conflict markers, test code, commit

---

## Conflict Detection & Flow (`#bonjour` §1)

```
#bonjour execution order:

§1.1: git fetch origin
§1.2: Compare HEAD vs origin/main
      IF retard detected:
        → git pull origin main
        → IF conflicting files found:
             FOR EACH conflicting file:
               - Identify category (SYNC, LOCAL, AUDIT, APPEND, CODE)
               - Apply resolution rule (THEIRS, OURS, MERGE, HUMAN)
               - git add <resolved file>
             - If human action needed: STOP and list files
          → git status  # Verify 0 unmerged paths
§1.3: PASS — continue with §2
```

---

## Priority Matrix (Quick Reference)

| Category | Conflict Rule | Action | Auto? | Human? |
|----------|---------------|--------|-------|--------|
| SYNC | Remote wins | `git checkout --theirs` | ✅ Yes | ❌ No |
| LOCAL | Local wins | `git checkout --ours` | ✅ Yes | ⚠️  Review needed |
| AUDIT | Local wins | `git checkout --ours` | ✅ Yes | ⚠️  Review needed |
| APPEND | Merge both | Manual append | ❌ No | ✅ Yes |
| CODE | Human decides | Manual merge | ❌ No | ✅ Yes |

---

## Conflict Prevention Best Practices

1. **Avoid Simultaneous Edits** : If 2 PCs editing same LOCAL file, use comment tags:
   ```
   [PC-A: account section]
   [PC-B: deployment section]
   ```

2. **Designate Section Owners** : `operating-rules.md` sections assigned per PC:
   - PC-A: §1-§3 (Account, Deployment)
   - PC-B: §4+ (Custom rules)

3. **Session Timing** : Stagger sessions if possible — PC-A 22:00-23:00, PC-B 09:00-10:00

4. **Frequent Pulls** : Pull before any long editing session to minimize divergence

5. **Tag Changes** : Use commit messages with `[PC-A]` or `[PC-B]` prefix for traceability

---

## Error Scenarios

### Scenario 1: SYNC File Conflict (Should Not Happen)

**Cause** : Local `_governance/core/copilot-instructions-commun.md` edited on PC-A, same file edited on PC-B  
**Fix** : Accept remote, investigate why local was edited (should only change via sync-governance script)  
**Prevention** : SYNC files are READONLY on individual PCs — edit in `_governance/` root only

### Scenario 2: LOCAL File Conflict (Expected)

**Cause** : PC-A modified `.github/copilot-instructions.md` account section, PC-B modified same section differently  
**Fix** : `git checkout --ours` then review with team — whose account declaration is correct?  
**Prevention** : Assign LOCAL sections per PC, don't overlap edits

### Scenario 3: Append-Only Conflict (Expected)

**Cause** : `session-log.md` has new entries from PC-A and PC-B in same session window  
**Fix** : Merge (append) both entries preserving order  
**Prevention** : None — this is normal operation

### Scenario 4: Code Conflict (Common)

**Cause** : `Code.js` edited for feature X on PC-A, feature Y on PC-B  
**Fix** : Manual merge — preserve both features, resolve marker conflicts  
**Prevention** : Use feature branches or coordinate PC usage per project

---

## Automation Scripts

### `resolve-conflicts-auto.ps1` — Automatic Resolution

```powershell
param(
    [string]$conflictStrategy = "standard"  # or "aggressive" for SYNC-priority
)

# 1. Detect all conflicting files
$conflicts = git diff --name-only --diff-filter=U

foreach ($file in $conflicts) {
    $category = Get-FileCategory $file  # Determine SYNC/LOCAL/AUDIT/APPEND/CODE
    $rule = Get-ConflictRule $category
    
    if ($rule -eq "REMOTE") {
        git checkout --theirs $file
        git add $file
        Write-Host "[SYNC] $file — remote accepted"
    } elseif ($rule -eq "LOCAL") {
        git checkout --ours $file
        git add $file
        Write-Host "[LOCAL] $file — local retained (review needed)"
    } elseif ($rule -eq "MERGE") {
        Invoke-AppendMerge $file
        git add $file
        Write-Host "[MERGE] $file — entries appended"
    } else {
        Write-Host "[MANUAL] $file — human mediation required"
    }
}

# 2. Verify no unmerged paths remain
$remaining = git diff --name-only --diff-filter=U
if ($remaining.Count -gt 0) {
    Write-Host "⚠️  Unresolved conflicts remain:" -ForegroundColor Yellow
    $remaining | ForEach-Object { Write-Host "  $_" }
    exit 1
}

Write-Host "✅ All conflicts resolved"
git status
```

### `verify-merge-completeness.ps1` — Post-Merge Validation

```powershell
# Verify append-only files have both versions merged
$appendOnlyFiles = @("_governance/session-log.md", "docs/project/decision-log.md")

foreach ($file in $appendOnlyFiles) {
    if (Test-Path $file) {
        $local = git diff HEAD^..HEAD -- $file
        if ($local -match "<<<<<<<") {
            Write-Host "⚠️  Conflict markers remain in $file" -ForegroundColor Yellow
            exit 1
        }
    }
}

Write-Host "✅ Merge completeness verified"
```

---

## References

- **Related** : `pc-identity.md` (PC identification for cross-PC tracking)
- **Related** : `session-log.md` format (tracks which PC made each change)
- **Used by** : `#bonjour` §1 (conflict detection flow)
- **Used by** : `resolve-conflicts-auto.ps1` (automation)
