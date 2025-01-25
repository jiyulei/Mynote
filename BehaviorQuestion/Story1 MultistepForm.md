
#behavior
### Most Complicated Component?
### 你曾经解决过一个复杂的技术挑战吗？
### 你是否创建过对业务产生重大影响的功能？
### 你如何处理跨组件的状态共享问题？（useContext， useState）
### 你如何确保代码的可维护性和扩展性？ （useForm独立管理， zod验证规则易于扩展）
### 你是否参与过一个需要跨团队协作的项目？（API Schema match with Form）
### 你曾经在时间压力下交付过一个功能吗？
### 举个例子说明你如何使用技术优化用户体验？（validation， dynamic fields）
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

**How to ensure data** **integrity**: (每一步完整再更新parent state)
I ensured data integrity by only allowing updates to the parent state when the current step passed validation. Each step had its own `onSubmit` function tied to React Hook Form’s `handleSubmit`.

**Handle Page Refresh(防止表单数据丢失)：**
To handle page refreshes, I considered using [[localStorage and sessionStorage]] to temporarily save the state.
