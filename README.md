# 求助 / Help wanted: DGX/Raven 授权绑定、安全采用与持续调度审查

## 中文

这是用户项目的技术求助，由参与工作的 OpenAI 助手起草，**不是 OpenAI 官方声明**。截至 2026-11-02，助手尚未交付约定的整套 DGX/Raven 系统；已有部分实现和测试，但不能把这些等同于系统已完整恢复、部署或验收。希望有相关经验的开发者帮助复核设计和收敛剩余工作。

### 已完成与边界

- 已实现离线候选：显式 owner 确认后端、严格 JSON HTTP 适配层、现有 canonical verifier 接入点，以及完整作用域摘要的生产端/消费端校验。
- 初始阶段 45 项合成后端用例和 12 项原有消费端单元测试通过（合计 57 项）。后续摘要/UI 回归及异步竞争测试也已通过；Python/JavaScript 作用域摘要一致性已验证。
- 原 DGX 补丁适用性检查通过，但检查不代表已应用。新行为测试中的原认证与准入接口是 mock，尚不是实际 DGX 认证、账本和完整任务链路验收。另一个同系列助手做过独立复核，不是外部人工审计。
- 手机 UI **仅为离线预览**，不提交真实确认，产物不能充当授权；真实浏览器渲染验证尚未通过。
- 最新 intake 源文件私有传输已成功，138 项源文件输入已补齐；DGX 独立 v5 构建尚未启动，目前未上线。既有 V4 保持不动。

上述进展不表示原 Raven 的全部业务不可用，也不表示已有成果被清空。这里不公开私有源码、账号、任务标识、资金信息、凭据或原始聊天。

### 希望获得的具体帮助

1. **可信确认与绑定设计审查：**如何在现有已配对 owner 身份链内，将人明确确认的完整作用域可靠绑定到 task、demand、parent 和受审查 runtime manifest，并在消费时再次校验？如何证明篡改、重放、过期、配对变更及异步取消均安全拒绝？当前文件信任模型不防御被攻陷的同 UID 服务，摘要也不等于签名或批准。
2. **最小安全采用与恢复：**请建议从离线候选走向可回退采用所需的最小证据和顺序，尤其是源文件冻结清单、真实认证/准入的获准无费用测试、生产确认 UI，以及原服务恢复验证。旧授权过期仍应 HOLD，新的费用确认不能续期旧授权或重置额度。
3. **持续调度缺口：**整套系统的持续调度闭环尚未交付验收。希望复核手机驾驶舱与 Temporal 任务服务之间的状态契约、幂等、重启恢复和可观测性，并指出还需要哪些脱敏证据才能定位实际缺口。

欢迎先提供架构审查问题、验收清单或最小可复现实验建议。无需访问生产系统；不请求绕过权限、伪造 owner 批准、增加模型调用或以真实付费任务试错。后续材料只会在用户明确授权及脱敏后提供。

## English

This is a technical help request for a user's project, drafted by the OpenAI assistant involved in the work. **It is not an official OpenAI statement.** As of 2026-11-02, the assistant has not delivered the agreed complete DGX/Raven system. Partial implementations and tests exist, but they do not establish complete recovery, deployment, or acceptance.

**What exists:** an offline explicit-owner-confirmation backend, strict JSON HTTP adapter, existing canonical-verifier integration seam, and producer/consumer scope-digest checks. The initial phase passed 45 synthetic backend cases plus 12 existing consumer unit tests (57 total). Subsequent digest/UI regression and asynchronous-race tests passed, including Python/JavaScript scope-digest parity. Original DGX patch applicability was checked, not applied. Authentication/admission are mocked in the new behavioral tests; real DGX authentication, ledger integration and end-to-end acceptance remain unverified. A separate same-family assistant reviewed the candidate; this was not an external human audit.

The phone UI is **offline preview only**, cannot submit real consent, and has not passed rendered-browser QA. Private transfer of the latest intake source succeeded, completing the set of 138 source inputs. The isolated DGX v5 build has not started; v5 is not deployed. Existing V4 remains untouched. None of this asserts that all original Raven services are unavailable or that existing work was erased.

**Review requested:**

1. Bind explicit, human-visible scope to the authenticated owner, task, demand, parent and reviewed runtime manifest; revalidate on consumption and fail closed on tampering, replay, expiry, pairing changes and asynchronous cancellation. The current file-trust model does not protect against a compromised same-UID service; a digest is not a signature or approval.
2. Define minimal evidence and a rollback-safe adoption/recovery sequence: frozen-source inventory, authorized no-charge real authentication/admission tests, production consent UI and original-service recovery checks. Expired authority must stay HOLD; new fee consent must not renew old authority or reset allowances.
3. Review the still-unaccepted continuous-scheduling loop between the phone cockpit and Temporal task service: state contracts, idempotency, restart recovery and observability. What sanitized evidence is needed to identify the remaining gaps?

Architecture-review questions, acceptance checklists and minimal reproducible test suggestions are welcome. No production access is requested. Please do not suggest bypassing permissions, fabricating owner consent, adding model calls or experimenting with real paid tasks. Private source, account details, task identifiers, financial data, credentials and raw conversations are excluded; further materials require explicit user authorization and redaction.
