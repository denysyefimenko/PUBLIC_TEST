Develop Chrome Extension

- **Owner**: Denys Yefimenko

### Purpose

- Explain and capture knowledge needed to quick start Chrome Extension development
- What's out of scope
  - Extension development for browsers other than Chrome or based on Chromium

### DOK4 - SPOV / New Knowledge

- React should be mandated as the only acceptable UI framework for Chrome extensions despite performance concerns; developers who insist on vanilla JavaScript are creating technical debt disguised as optimization - their extensions will inevitably become unmaintainable as features grow, costing organizations 3x more in long-term development hours than any initial performance gains.
- TypeScript should be mandatory for all Chrome extension projects with strict compiler settings enabled from day one as it prevents the runtime errors that plague JavaScript extensions, reduces QA cycles, and integrates seamlessly with modern build tools - making any short-term learning curve cost negligible compared to long-term maintenance benefits.
- Always implement at minimum a bundler and `transpiler` in Chrome extension projects, even for vanilla JavaScript codebases, as they reduce distribution size by 40% and eliminate common runtime errors that waste development time [See Insight: Modern Chrome extension tech stack].

### DOK3 - Insights

- The architectural shift from background scripts to service workers in Manifest V3 represents a fundamental trade-off between persistent state management and improved security that forces developers to adopt event-driven programming patterns.
- Chrome extension's zip-based distribution model creates an inherent tension between external resource dependencies and security, pushing developers toward self-contained architectures and leveraging build tools that prioritize reliability over code reuse - a constraint that shapes the entire development approach.
- Modern Chrome extension development has converged on a tech stack (`TypeScript` , `Vite` , `React` ) that optimizes developer productivity at the cost of some runtime performance - a trade-off that reflects the industry's shift toward faster iteration over perfect optimization.

### Experts

- Nikolaos Pantelaios [https://www.linkedin.com/in/nikolaos-pantelaios-891b99130](https://www.linkedin.com/in/nikolaos-pantelaios-891b99130)
  - AI Research Scientist @ Meta
  - Manifest V3 Unveiled: Navigating the New Era of Browser Extensions [https://arxiv.org/abs/2404.08310v1](https://arxiv.org/abs/2404.08310v1)
  - This expert is relevant because he authored a paper that provides a comprehensive, data-driven analysis of Manifest V3's adoption and security impact, equipping new Chrome Extension developers with critical insights into the current development framework and its implications.
- Nathan Bierema [https://github.com/Methuselah96](https://github.com/Methuselah96)
  - Technical lead at [@ParagonTruss](https://github.com/ParagonTruss). Maintainer of redux-devtools and co-maintainer of react-select and immutable-js.
  - This expert is relevant because he maintains popular chrome extension `Redux DevTools` used for debugging application's state changes in React.

### DOK1 and DOK2 - Knowledge Tree and Sources

- Knowledge Area: Extension Capabilities
  - Source: Chrome Extension Architecture Documentation
    - DOK2 Summary: What is possible with chrome extensions. Interaction with Dom, Shadow Dom, embedding into the existing page, 
      - DOK1 Fact: Chrome extensions can access and modify web page content using content scripts
      - DOK1 Fact: Extensions can use `chrome.storage` API to store and retrieve user data persistently
      - DOK1 Fact: Chrome extensions can communicate between components using message passing
- Knowledge Area: Extension Structure
  - Source: Chrome Extension Architecture Documentation
    - DOK2 Summary: Chrome extensions follow a component-based architecture with specific required and optional files
      - DOK1 Fact: The manifest.json file is required and defines the extension's capabilities and limitations
      - DOK1 Fact: Content scripts can access and modify web pages but have limited access to extension APIs
      - DOK1 Fact: Service workers (replacing background scripts in Manifest V3) handle events and maintain state
      - DOK1 Fact: `manifest.json` can specify `options_page` - a component which gets presented to user when they interact with Options context menu
- Knowledge Area: Publishing Extensions
  - Source: Chrome Web Store Developer Documentation
    - DOK2 Summary: The Chrome Web Store has specific requirements that must be met before an extension can be published
      - DOK1 Fact: Extensions must include a detailed privacy policy if they collect any user data. This privacy policy should be easily accessible by any user of extension (e.g. hosted on website)
      - DOK1 Fact: The review process typically takes 2-3 business days for new extensions
      - DOK1 Fact: Manifest V3 extensions receive priority review compared to Manifest V2
- Knowledge Area: Permissions
  - Source: Chrome Web Store Developer Documentation
    - DOK2 Summary: Chrome extensions use a permission-based security model that requires explicit declaration of all capabilities
      - DOK1 Fact: Permissions must be declared in the manifest.json file under the "permissions" array
      - DOK1 Fact: Users are shown all requested permissions before installation and can reject extensions that request excessive permissions
      - DOK1 Fact: Manifest V3 introduced more granular permissions and removed some powerful but potentially dangerous capabilities
      - DOK1 Fact: Static `content_scripts` defined in `manifest.json` do not imply `scripting` permission to be required. Only `chrome.scripting` API's used is when that permission should be specified
      - DOK1 Fact: Review may and will fail not only when required permission is mission, but when excessive permissions not used by extension are requested
      - DOK1 Fact: Chrome Web Store documentation uses color codes (Blue Argon, Yellow Magnesium, Purple Potassium, etc.) to represent different types of review failures and warnings, as documented in their developer guidelines [[link to source](https://developer.chrome.com/docs/webstore/troubleshooting)].
- Knowledge Area: State Management in V3
  - DOK2 Summary: Manifest V3 fundamentally changes extension state management by replacing persistent background scripts with non-persistent service workers. This architectural shift improves security and performance but requires developers to adopt event-driven programming patterns and alternative state persistence strategies like storage APIs, creating a trade-off between development simplicity and browser resource efficiency. [[source](https://developer.chrome.com/docs/extensions/develop/migrate)] 
    - DOK1 Fact: Extension Service Workers are short living entities serving to handle extension events [[source](https://developer.chrome.com/docs/extensions/develop/concepts/service-workers#lifecycle)]
    - DOK1 Fact: Extension Service Workers are **non-persistent** — they spin up on events and shut down shortly after. This breaks the older pattern of long-lived background scripts holding state
    - DOK1 Fact: Easiest way to persist state is by using `chrome.storage` API, which exists in two flavors: `chrome.storage.local` and `chrome.storage.sync` - one for per-device, and second for synced data
    - DOK1 Fact: We still can leverage in-memory state in service worker - best for short living state, while service worker instance is alive
    - DOK1 Fact: Use `IndexedDB` for complex data storage
    - DOK1 Fact: Avoid storing critical data in global variables of service worker - it won't survive suspension
    - DOK1 Fact: Artificially maintaining service worker alive is against MV3 principles
- Knowledge Area: Service Workers in V3
  - DOK2 Summary TBD
    - DOK1 Fact: The only way to update service worker in extension is to publish new version of the extension. For security reasons Manifest V3 [does not support](https://developer.chrome.com/docs/extensions/develop/migrate/improve-security#remove-remote-code) remotely-hosted code. Service worker must be part of the extension package.
- ------------------------------------- WIP ---------------------------------
- Due to Content Security Policy (CSP) restrictions, most pages (especially major sites like Google) block inline scripts.
