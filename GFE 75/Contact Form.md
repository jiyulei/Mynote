#gfe75
[题目](https://www.greatfrontend.com/interviews/study/gfe75/questions/user-interface/contact-form/react)

注意事项： 
1. `html-for`是react里的，必须匹配`input`的`id` 
2. `<input type="submit" />` 会变成button
3. 表单的三个属性，`onSubmit` `method` `action`
``` javascript
import submitForm from './submitForm';

export default function App() {
  return (
    <form
      // Ignore the onSubmit prop, it's used by GFE to
      // intercept the form submit event to check your solution.
      onSubmit={submitForm}
      method="POST"
      action="https://www.greatfrontend.com/api/questions/contact-form">
      <div>
        <label html-for='fname'>Name</label><br/>
        <input type="text" id="fname" name="name"/>
      </div>

      <div>
        <label html-for='femail'>Email</label><br/>
        <input type="email" id="femail" name="email"/>
      </div>

      <div>
        <label html-for='fmessage'>Message</label><br/>
        <textarea type="text" id="fmessage" name="message"/>
      </div>
   
      <div>
        <input type="submit" /> 
      </div>
    </form>
  );
}

```