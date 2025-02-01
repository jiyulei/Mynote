
#behavior
### Most Complicated Component?
### 你曾经解决过一个复杂的技术挑战吗？
### 你是否创建过对业务产生重大影响的功能？
### 你如何处理跨组件的状态共享问题？（useContext， useState）
父组件使用useState管理每个步骤状态，用useContext共享状态，确保数据在步骤间一致。
### 你如何确保代码的可维护性和扩展性？ （useForm独立管理， zod验证规则易于扩展）
React hook form 提供了fields register & from state management， 避免重新渲染。zod schema 可以更改，已于扩展。
### 你是否参与过一个需要跨团队协作的项目？（API Schema match with Form）/ 如果后端暂时无法提供完善的API，你如何在前端阶段先行迭代?
使用json-server或自己写mock server以及使用Swagger等方式模拟后端
### 你曾经在时间压力下交付过一个功能吗？
为了按时交付，我优先处理了核心功能，如动态字段渲染和基本验证。然后，我逐步添加了更复杂的验证逻辑和错误处理。
break task into small tasks, core funtionality --> eg. dynamic steps, and basic validation.
Get core functionality to production. Then complicated but not high priority task like dynamic schema.
### 举个例子说明你如何使用技术优化用户体验？（validation， dynamic fields）

### 在与其他团队或同事合作时，如果在需求或实现方式上有分歧/矛盾，你是如何处理的？
**场景**：  
在开发多步骤表单的过程中，我们（前端团队）最初与后端团队约定了一个API Schema，用于提交信用卡申请数据。例如，表单中的“邮寄地址”字段在接口中被定义为 `shippingAddress`。然而，在开发中期，后端团队因内部架构调整，提出需要将字段名改为 `deliveryAddress`，并要求在部分场景下将地址拆分为独立的 `street`、`city` 和 `postalCode` 字段。这一变更导致前端表单的数据结构与接口不兼容，且需要额外的工作量调整动态字段的逻辑。

**处理过程**：

1. **明确分歧点**：  
    我首先与后端团队同步上下文，确认变更原因。了解到他们的调整是为了统一其他服务的字段命名规范，并支持后续的地址校验功能。
    
2. **评估影响**：
    - 前端需要修改表单的数据合并逻辑，将原单一字段 `shippingAddress` 拆分为多个子字段。
    - 动态字段的验证规则（例如，当用户选择“实体卡”时，`street`、`city` 必须必填）需要同步更新。
    - 项目时间紧迫，直接接受变更可能延迟交付。
3. **提出协作方案**：  
    我提出了两个方案：
    - **方案1**：后端暂时兼容旧字段名 `shippingAddress`，同时支持新字段名 `deliveryAddress`，过渡期为2周，前端再逐步迁移。
    - **方案2**：前端立即适配新字段，但需要后端配合调整接口返回的错误格式（例如，将 `deliveryAddress.street` 的错误映射到前端的 `street` 字段）。
4. **达成共识**：  
    经过讨论，后端团队同意方案2，因为他们的架构调整已接近完成，可以优先保证接口错误格式的兼容性。同时，他们承诺在文档中明确新字段的校验规则，并协助前端测试。
5. **执行与验证**：
    - 我调整了表单的数据合并逻辑，将 `shippingAddress` 拆解为 `street`、`city` 和 `postalCode`，并通过 `useForm` 的 `setValue` 方法动态填充这些字段。
    - 使用 `zod` 的嵌套 Schema 对新字段进行验证，确保与后端规则一致。
    - 与后端团队联合测试，确保错误映射（如 `deliveryAddress.street is required` 能正确关联到前端表单项）正常工作。
**结果**：  
最终，我们按时交付了功能，且接口变更未对用户体验造成影响。通过这次协作，我也学到了两点：
1. **提前对齐变更风险**：在需求初期明确字段命名和结构的重要性。
2. **灵活设计数据层**：将表单数据与接口数据解耦，通过中间层（如 `transform` 函数）转换数据格式，降低未来变更的影响。

---

At T company, I built a multistep form using `React Hook Form` and the `useForm` hook. The purpose of the form was to allow admins to apply for new credit cards on behalf of other users. The form consisted of multiple steps. For example, in the first step, the admin would choose whether the card was a physical card or a virtual card. In the second step, they would fill out the user’s basic information, which included validation. For validation, I used the `zod` library to define schemas for each step of the form.

One interesting aspect of the form was that the fields in later steps depended on the choices made in earlier steps. For instance, in a subsequent step, the admin would specify the card’s purpose, such as gas, grocery, etc., and the fields displayed were _**dynamic**_ based on the previous inputs. To handle this, I implemented state sharing across the steps using [[useContext]]. In the parent component, I used [[useState]] to store the state of each step, ensuring that the data persisted when navigating between steps. When moving to the next step, I updated the parent state with the current form data, which was then accessible in the later steps.

At last, I merged all step data from the parent state into a single payload and sent it to the backend via an API call. If the **backend returned an error,** I mapped it to the specific step and field, so the user could easily identify and correct the issue.

## 结果：
This multistep form made the credit card application process super smooth for admins. It simplified the workflow, cut down on unnecessary steps, and made everything **faster** and **easier** to use. The dynamic fields and validations helped reduce mistakes, so the whole process became way more efficient.

---

**如何动态渲染(dynamic render fields):**
I used conditional rendering based on the state shared via `useContext`. For example, if the admin selected ‘physical card’ in the first step, the next step would display fields related to shipping information.

Reason `useContext` not `Redux`: lightweighted and sufficient for sharing state aross small components.

**如何优化：**
_(UX) aspect:_
I optimized the user experience by ensuring that form data persisted across steps using the parent state. If the admin navigated back to a previous step, their input was pre-filled. I also implemented client-side validation to provide immediate feedback for errors, reducing the need for additional API calls.

**Why React Hook Form:**
1. Efficient Field Registration: The `register` function binds form fields to the `formState` automatically, don't need to manually handle `onChange`, `onBlur`, and `value`.
2. Built-in Performance Optimizations: Avoids unnecessary re-renders by isolating the form field's state.
3. Seamless Integration with Zod: React Hook Form integrates smoothly with `zod` via the `zodResolver`
4. Validation errors were captured in `formState.errors` and displayed dynamically.
5. Ease of State Management: syncing data to the parent component.

**Why Zod**: 
[[zod example]]
Because it integrates seamlessly with `React Hook Form` and allows for declarative schema definition with excellent TypeScript support. For dynamic validation, I updated the schema conditionally based on the shared state.

For form *validation*, I ensured each step had independent validation using useForm and `zod`. Errors were displayed step-by-step, so the user couldn’t proceed until the current step was valid.

`zod`: I can set required / optional fields.  Display or customize errors.

复杂的验证逻辑：
Use `refine`, `when`, and condition check in zod with formstate in react hook form together to update validator.


**How to ensure data** **integrity**: (每一步完整再更新parent state)
I ensured data integrity by only allowing updates to the parent state when the current step passed validation. Each step had its own `onSubmit` function tied to React Hook Form’s `handleSubmit`.

**Handle Page Refresh(防止表单数据丢失)：**
To handle page refreshes, I considered using [[localStorage and sessionStorage]] to temporarily save the state.
