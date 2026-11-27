# SDC Studio — Visual Design System

**Status:** Design proposal for owner review  
**Work item:** P2-08  
**Purpose:** Define visual language, reusable components, tokens, and interface states for consistent design and later frontend implementation.

## 1. Design direction

**Direction:** Professional developer workspace with a calm, information-dense but readable interface.

The experience should feel like a focused combination of a modern AI chat workspace and a developer IDE. It should not look like a generic admin dashboard or a marketing website.

Principles:
- Conversation-first.
- Developer-friendly information density.
- Strong hierarchy between chat, activity, code, preview, and evidence.
- Quiet surfaces; use accent colors sparingly.
- Clear state indicators for running, failed, blocked, approved, and completed work.
- High contrast and readable code/output.
- Responsive layout without squeezing desktop panes onto mobile.
- Consistency over decorative animation.

## 2. Theme proposal

### Default theme

A dark neutral developer theme is proposed:
- Deep neutral background for global shell.
- Slightly lighter sidebar and work surfaces.
- Distinct surface elevation for cards, menus, dialogs, and panels.
- High-contrast primary text.
- Muted secondary text.
- One restrained brand accent for primary actions/focus.
- Semantic colors reserved for status: success, warning, error, and informational.

### Light theme

A corresponding light theme should use:
- Warm/neutral light background.
- White or near-white working surfaces.
- Dark primary text and accessible muted text.
- Same semantic status mapping and accent identity.

### Branding decision

The exact SDC logo, accent hue, and final color values are **provisional** until owner review. Keep branding tokens centralized so the palette can be changed without redesigning components.

Do not use bright colors for every card or category. Status colors should communicate meaningful state only.

## 3. Typography

Proposed type roles:
- **Interface:** Inter or a comparable highly legible sans-serif.
- **Code/terminal:** JetBrains Mono or a comparable monospace font.
- **Display headings:** restrained weight/size; avoid oversized marketing typography in the work area.

Type scale proposal:
| Token | Intended use |
|---|---|
| Display | Home greeting or project name, sparingly |
| H1 | Page title |
| H2 | Main section heading |
| H3 | Panel/card heading |
| Body | Conversation and primary content |
| Body small | Metadata, secondary labels |
| Caption | Timestamps, helper text and secondary status |
| Code | File contents and terminal output |

Use readable line height for conversation text and denser line height for code/terminal. Do not use tiny low-contrast metadata to save space.

## 4. Spacing and layout tokens

Use a consistent 4px-based spacing scale:
- 4px: icon/text micro-gap.
- 8px: compact control spacing.
- 12px: small content gap.
- 16px: standard control/card padding.
- 24px: section spacing.
- 32px: large section spacing.
- 40px/48px: page-level separation where needed.

Layout widths and heights are defined by screen design and responsive behavior, not arbitrary per-component values.

## 5. Surface and border language

- Global shell: one base surface.
- Sidebar and context panel: subtly differentiated surfaces.
- Primary content: main surface with clear boundaries only where useful.
- Cards: restrained border and subtle surface separation.
- Menus/popovers: elevated surface with clear focus and dismissal behavior.
- Dialogs: clear heading, explanation, action area, and focus management.
- Avoid excessive nested cards and heavy shadows.

## 6. Color token categories

Define semantic tokens rather than hardcoding raw colors in components.

| Token family | Example semantic tokens | Use |
|---|---|---|
| Background | bg-canvas, bg-sidebar, bg-surface, bg-elevated | Page and panel surfaces |
| Text | text-primary, text-secondary, text-muted, text-inverse | Text hierarchy |
| Border | border-subtle, border-default, border-strong | Separation and focus |
| Accent | accent-primary, accent-hover, accent-subtle | Primary actions and focus |
| Success | status-success, status-success-bg | Passed/completed |
| Warning | status-warning, status-warning-bg | Warnings/approval required |
| Danger | status-danger, status-danger-bg | Failed/destructive |
| Info | status-info, status-info-bg | Running/neutral information |
| Disabled | text-disabled, bg-disabled | Unavailable controls |

Final hex values and contrast pairing are a design acceptance item, not locked in this draft.

## 7. Iconography

- Use one consistent outline icon family.
- Icons should be recognizable at common interface sizes.
- Pair less familiar icons with labels/tooltips.
- Avoid using status color alone; combine icon, text, and semantic state.
- Do not use emoji as the primary control iconography.

## 8. Component inventory

### Navigation
- App logo/brand.
- Sidebar item.
- Project list item.
- Conversation list item.
- Breadcrumb/project header.
- Tab/segmented workspace navigation.
- Collapsible context panel.

### Chat
- Message bubble/stream block.
- User message.
- Assistant response.
- Code block with copy action.
- Citation/source link.
- Composer.
- Attachment chip.
- Drag/drop upload area.
- Mode selector.
- Model selector.
- Autonomy selector.
- Send/stop/retry action.

### Agent execution
- Task status badge.
- Task plan card.
- Step row.
- Activity event.
- Tool invocation detail.
- Approval request.
- Progress indicator.
- Error/blocked state panel.

### Developer workspace
- File tree item.
- Code/file viewer.
- Diff viewer.
- Terminal block.
- Preview toolbar/frame.
- Viewport selector.
- Test result row.
- Screenshot thumbnail/gallery.
- Console/network event row.

### Reporting and settings
- Report section.
- Metric/result summary.
- Provider card.
- GitHub connection card.
- Permission summary.
- Secret configured indicator (masked).
- Empty state.
- Confirmation dialog.
- Toast/inline alert.

## 9. Component states

Every interactive component defines:
- Default.
- Hover.
- Focus-visible.
- Active/pressed.
- Disabled.
- Loading where applicable.
- Error where applicable.
- Selected/expanded where applicable.

Task-related components also distinguish:
- Draft.
- Queued.
- Running.
- Awaiting approval.
- Completed.
- Completed with issues.
- Blocked.
- Failed.
- Cancelled.

## 10. Form design

- Persistent visible labels for important fields.
- Inline validation close to the relevant field.
- Explain why a field is needed.
- Avoid destructive defaults.
- Keep submit action distinct from cancel.
- Preserve user-entered data after recoverable errors.
- Do not expose full credential values after saving.

## 11. Status and evidence styling

- Running: neutral/informational indicator plus explicit text.
- Completed: success indicator plus actual completion label.
- Completed with issues: warning state with issue count/summary.
- Blocked: warning state with blocker source.
- Failed: error indicator and actionable diagnostic.
- Not run: neutral, never green.
- Approval required: visible warning and specific action description.

A green overall task status must not conceal failed or skipped tests.

## 12. Accessibility requirements

- WCAG 2.2 AA is the target for primary operator workflows.
- Text and controls must meet required contrast.
- All interactive controls must be keyboard accessible.
- Focus is clearly visible.
- Dialogs manage focus and restore it on close.
- Status updates are announced appropriately without excessive live-region chatter.
- Color is never the only state signal.
- Reduced-motion preference is respected.
- Tooltips are not the only place where essential information exists.

## 13. Motion

Use motion only to communicate:
- Sidebar/context panel transitions.
- Message/stream arrival.
- Task progress/state changes.
- Dialog/menu transitions.

Keep animations short and restrained. Avoid background effects, looping decoration, or motion that interferes with code reading.

## 14. Density modes

The owner-selected baseline is **Comfortable**. **Compact** remains available as an optional global workspace preference.

- **Comfortable (default):** more breathing room for chat, reports, file rows, settings and activity.
- **Compact (optional):** tighter row and section spacing to show more files, tool events, test rows and terminal output.

Use one predictable global density selection across workspace surfaces. Individual surfaces may adapt their layout to the viewport and task, but Phase 2 does not introduce independent per-panel density overrides. Density must not reduce text readability, minimum target sizes, focus visibility, or accessibility.

## 15. Design tokens handoff

The design handoff should list:
- Token name.
- Semantic purpose.
- Light value.
- Dark value.
- Contrast validation.
- Components that consume it.

The actual CSS variables/tailwind configuration are implementation work and must not be created during Phase 2.

## 16. Owner review items

**Resolved owner decisions:**
- Both Light and Dark themes are required. First visit follows the operating-system preference; a manual selection is remembered and restored on later visits.
- Comfortable is the default workspace density. Compact remains an optional density preference.

**Still requiring owner review:**
- Final SDC brand accent/logo direction.
- Whether project workspace navigation should use horizontal tabs or a secondary navigation rail.
- Final visual direction and the clickable prototype before Phase 2 acceptance.

These visual decisions remain within the Phase 2 design scope and do not change the Phase 0 product foundation.


## 17. Owner-selected defaults

The operator selected the following preferences for SDC Studio:

- **Theme strategy:** Both Light and Dark themes are available.
- **Theme persistence:** On first visit, use the operating-system theme preference until the operator manually selects Light or Dark. Remember the last manual selection and restore it on the next visit.
- **Workspace density:** Comfortable is the default. Compact is an optional density setting for operators who want more rows, messages, files or activity entries visible.

### Density behavior

Comfortable:
- More vertical spacing in chat and reports.
- Easier separation of task/activity events.
- Larger row padding in file lists and settings.
- Best default for everyday use and long sessions.

Compact:
- Reduced spacing and row height.
- More files, tool events, test rows and terminal output visible.
- Best for intensive development and evidence review.

Compact must not shrink text below accessible sizes or remove labels/status information. Density changes layout spacing, not semantic hierarchy, color meaning or component behavior.

These are owner-selected preferences for the product design. They are not a choice of brand accent or final logo.
