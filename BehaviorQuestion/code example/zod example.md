``` javascript
import React from "react";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";

export default function ZodErrorDemo() {
  const {
    register,
    handleSubmit,
    formState: { errors }, // 获取 errors 对象
  } = useForm({
    resolver: zodResolver(formSchema), // 使用 Zod 验证
    defaultValues: {
      firstName: "",
      age: "",
      email: "",
    },
  });

  const onSubmit = (data) => {
    console.log("Validated Data: ", data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* First Name */}
      <div>
        <label>First Name</label>
        <input {...register("firstName")} />
        {errors.firstName && <p>{errors.firstName.message}</p>} {/* 错误提示 */}
      </div>

      {/* Age */}
      <div>
        <label>Age</label>
        <input type="number" {...register("age", { valueAsNumber: true })} />
        {errors.age && <p>{errors.age.message}</p>} {/* 错误提示 */}
      </div>

      {/* Email */}
      <div>
        <label>Email</label>
        <input type="email" {...register("email")} />
        {errors.email && <p>{errors.email.message}</p>} {/* 错误提示 */}
      </div>

      <button type="submit">Submit</button>
    </form>
  );
}

```