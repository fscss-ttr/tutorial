# 07 — Projects

Put it all together.

Everything so far has been isolated concepts. These fifty projects use the full stack at once — tokens, defines, stores, arrays, events, imports — to build the components and pages you'll actually ship. Each project is self-contained: a brief, the FSCSS, the HTML it styles, and stretch goals.

## How to Work Through Them

1. Pick any project. They're ordered by difficulty, not by importance.
2. Read the brief, then try writing it yourself before peeking at the FSCSS.
3. Compile with the CLI (`fscss style.fscss style.css`) or run via the CDN runtime.
4. Do at least one stretch goal per project.

## The Projects

| # | Project | Difficulty |
|---|---------|-----------|
| [01](./01-button-system.md) | Button System | Beginner |
| [02](./02-card-component.md) | Card Component | Beginner |
| [03](./03-navigation-bar.md) | Navigation Bar | Beginner |
| [04](./04-hero-section.md) | Hero Section | Beginner |
| [05](./05-footer-design.md) | Footer Design | Beginner |
| [06](./06-form-elements.md) | Form Elements | Beginner |
| [07](./07-alert-boxes.md) | Alert Boxes | Beginner |
| [08](./08-modal-dialog.md) | Modal Dialog | Beginner |
| [09](./09-dropdown-menu.md) | Dropdown Menu | Beginner |
| [10](./10-progress-bar.md) | Progress Bar | Beginner |
| [11](./11-tabs-component.md) | Tabs Component | Beginner |
| [12](./12-accordion.md) | Accordion | Beginner |
| [13](./13-tooltip.md) | Tooltip | Intermediate |
| [14](./14-breadcrumb.md) | Breadcrumb | Intermediate |
| [15](./15-pagination.md) | Pagination | Intermediate |
| [16](./16-badge-tag.md) | Badges and Tags | Intermediate |
| [17](./17-data-table.md) | Data Table | Intermediate |
| [18](./18-avatar-group.md) | Avatar Group | Intermediate |
| [19](./19-loading-spinner.md) | Loading Spinner | Intermediate |
| [20](./20-toggle-switch.md) | Toggle Switch | Intermediate |
| [21](./21-image-gallery.md) | Image Gallery | Intermediate |
| [22](./22-pricing-table.md) | Pricing Table | Intermediate |
| [23](./23-newsletter-signup.md) | Newsletter Signup | Intermediate |
| [24](./24-timeline.md) | Timeline | Intermediate |
| [25](./25-notification-toast.md) | Notification Toast | Intermediate |
| [26](./26-sidebar-navigation.md) | Sidebar Navigation | Intermediate |
| [27](./27-search-bar.md) | Search Bar | Intermediate |
| [28](./28-testimonials.md) | Testimonials | Intermediate |
| [29](./29-dashboard-layout.md) | Dashboard Layout | Advanced |
| [30](./30-login-page.md) | Login Page | Advanced |
| [31](./31-signup-page.md) | Signup Page | Advanced |
| [32](./32-profile-card.md) | Profile Card | Advanced |
| [33](./33-chat-interface.md) | Chat Interface | Advanced |
| [34](./34-music-player.md) | Music Player | Advanced |
| [35](./35-video-player-ui.md) | Video Player UI | Advanced |
| [36](./36-calendar-widget.md) | Calendar Widget | Advanced |
| [37](./37-kanban-board.md) | Kanban Board | Advanced |
| [38](./38-stepper-wizard.md) | Stepper Wizard | Advanced |
| [39](./39-rating-stars.md) | Rating Stars | Advanced |
| [40](./40-file-upload.md) | File Upload | Advanced |
| [41](./41-comment-thread.md) | Comment Thread | Advanced |
| [42](./42-stats-cards.md) | Stats Cards | Advanced |
| [43](./43-landing-page.md) | Landing Page | Advanced |
| [44](./44-product-grid.md) | Product Grid | Advanced |
| [45](./45-shopping-cart.md) | Shopping Cart | Advanced |
| [46](./46-checkout-page.md) | Checkout Page | Advanced |
| [47](./47-blog-layout.md) | Blog Layout | Advanced |
| [48](./48-docs-layout.md) | Docs Layout | Advanced |
| [49](./49-portfolio-page.md) | Portfolio Page | Expert |
| [50](./50-admin-panel.md) | Admin Panel | Expert |

## The Recipe Every Project Follows

1. **Tokens** — colors, spacing, radius in `@fun` stores and `--` tokens.
2. **Blocks** — shared patterns as `@define` (or `str()` for the tiny ones).
3. **Data** — repeated items as `@arr` walks instead of copy-paste.
4. **Logic** — `@event` when a value depends on a mode or threshold.
5. **Compile or CDN** — then verify the output.

---

**Start: [01 — Project Button System](./01-button-system.md)**

---

*Written by [Omonire](https://github.com/Omonire)*