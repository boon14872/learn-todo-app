# Next.js Todo App Class - Building with Mock API

คลาสนี้เป็นคลาสที่ออกแบบมาเพื่อสร้างแอพพลิเคชั่น Todo โดยใช้ Next.js และ mock API จากบริการภายนอก จุดมุ่งหมายของคลาสนี้คือการให้ความรู้เบื้องต้นเกี่ยวกับ Next.js, การใช้งาน API และการดำเนินการ CRUD พื้นฐาน

## สิ่งที่ต้องมี

- Node.js และ npm ติดตั้งให้เรียบร้อย ([ดาวน์โหลด Node.js](https://nodejs.org/))
- ความรู้พื้นฐานเกี่ยวกับ React และ JavaScript
- บัญชีผู้ใช้งานบนเว็บไซต์ Mock API เช่น [MockAPI.io](https://mockapi.io/)
- โปรแกรมสำหรับเขียนโค้ด เช่น Visual Studio Code

## เทคโนโลยีที่ใช้

- **Next.js**: เฟรมเวิร์กสำหรับ React ที่มีคุณสมบัติการเรนเดอร์ที่อยู่บนเซิร์ฟเวอร์ และ API routes ที่ช่วยให้การสร้างแอพพลิเคชั่นเว็บเร็วและง่ายขึ้น
- **Mock API**: บริการที่ช่วยให้สร้าง API ทดลองที่ไม่จำเป็นต้องมีเซิร์ฟเวอร์จริง ทำให้สามารถทดสอบและพัฒนาแอพพลิเคชั่นได้โดยไม่ต้องพัฒนาฝั่งเซิร์ฟเวอร์

## มาเริ่มต้นกันเลย

### 1. Next.js คืออะไร?

    - Next.js เป็นเฟรมเวิร์กสำหรับ React ที่มีคุณสมบัติการเรนเดอร์ที่อยู่บนเซิร์ฟเวอร์ ทำให้เว็บไซต์ที่สร้างด้วย Next.js มีประสิทธิภาพสูงและเวลาโหลดเร็วขึ้น เขียนด้วย JavaScript และ React และสามารถใช้ TypeScript ได้ (แต่ในคลาสนี้จะใช้ TypeScript)

### 2. สร้างโปรเจค Next.js

- สร้างโปรเจค Next.js โดยใช้คำสั่ง `npx create-next-app` และเลือก TypeScript เป็นภาษาหลัก

```bash
npx create-next-app
```

หากขึ้น error npm ให้ทำการอัพเดท npm โดยใช้คำสั่ง

```bash
npm update -g npm
```

ให้ตอบคำถามต่างๆ ดังนี้:

- What is your project named? (ชื่อโปรเจค): `learn-todo
- Would you like to use TypeScript? (ใช้ TypeScript หรือไม่?): `Yes`
- Would you like to use ESLint? (ใช้ ESLint หรือไม่?): `No`
- Would you like to use Tailwind CSS? (ใช้ Tailwind CSS หรือไม่?): `Yes`
- Would you like to use `src/` directory? (ใช้ไดเร็กทอรี `src/` หรือไม่?): `Yes`
- Would you like to use App Router? (ใช้ App Router หรือไม่?): `Yes`
- Would you like to customize the default import alias (@/\*)? (ต้องการแก้ไขการนำเข้าค่าเริ่มต้นหรือไม่?): `No`

ทำการ cd เข้าไปในโปรเจคที่สร้างขึ้น

```bash
cd learn-todo
```

เปิดโปรเจคด้วย Visual Studio Code

```bash
code .
```

ทำการรันโปรเจคด้วยคำสั่ง

```bash
npm run dev
```

### 3. เตรียม Mock API

- สร้างบัญชีผู้ใช้งานบนเว็บไซต์ Mock API เช่น [MockAPI.io](https://mockapi.io/)
- สร้างโปรเจคใหม่บน Mock API และสร้างโมเดล `Todo` ที่มีฟิลด์ `title` และ `completed` โดยกำหนด `title` เป็น string และ `completed` เป็น boolean
  <!-- /resources/mock_api_create.png -->

  [![Create Mock API Project](/resources/mock_api_create.png)](https://mockapi.io/)

### 4. สร้างโมเดล Todo

- สร้างโมเดล `Todo` ในโฟลเดอร์ `models` โดยกำหนดฟิลด์ `title` เป็น string และ `completed` เป็น boolean

![Create Todo Model](/resources/model.png)

```typescript
// models/Todo.ts
export interface Todo {
  id: string;
  title: string;
  completed: boolean;
}
```

<!-- ทำการลบข้อมูลทั้งหมดใน page.ts ให้เหลือดังต่อไปนี้ -->

- ลบข้อมูลทั้งหมดในไฟล์ `src/app/index.tsx` ให้เหลือดังต่อไปนี้

```typescript
// pages/index.tsx
export default function Home() {
  return <main></main>;
}
```

ไฟล์นี้จะเป็นไฟล์หลักที่ใช้ในการแสดงผลหน้าเว็บเมื่อเราเข้าถึงหน้าหลักของเว็บไซต์

- ใน `src/app/globals.css` ให้เหลือดังนี้

```css
/* styles/global.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

ไฟล์นี้จะเป็นไฟล์ที่ใช้ในการกำหนด CSS สำหรับทั้งหน้าเว็บไซต์โดยรวม และใช้ Tailwind CSS ในการกำหนด CSS

<!-- ทำการออกแบบหน้าตาเว็บด้วย html + tailwind ในหน้าหลัก -->

- ออกแบบหน้าตาเว็บด้วย HTML และ Tailwind CSS ในไฟล์ `src/app/index.tsx` ดังต่อไปนี้ ตอนแรกเราจะสร้างฟอร์มเพื่อให้ผู้ใช้งานสามารถเพิ่ม Todo ได้

```typescript
// src/app/index.tsx
export default function Home() {
  return (
    <main>
      <form className="flex items-center justify-center mt-8">
        <input
          type="text"
          className="w-64 px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500"
          placeholder="What needs to be done?"
        />
        <button
          type="submit"
          className="ml-4 px-4 py-2 bg-indigo-600 text-white rounded-md shadow-sm hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2"
        >
          Add
        </button>
      </form>
    </main>
  );
}
```

- ในไฟล์ `src/app/index.tsx` ให้เพิ่มส่วนของการแสดงรายการ Todo ดังต่อไปนี้

```typescript
// src/app/index.tsx
export default function Home() {
  return (
    <main>
      <form className="flex items-center justify-center mt-8">
        <input
          type="text"
          className="w-64 px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500"
          placeholder="What needs to be done?"
        />
        <button
          type="submit"
          className="ml-4 px-4 py-2 bg-indigo-600 text-white rounded-md shadow-sm hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2"
        >
          Add
        </button>
      </form>
      <ul className="mt-8 bg-white shadow-lg rounded-lg w-3/4 mx-auto">
        <li className="flex items-center justify-between px-6 py-4 border-b border-gray-200 hover:bg-gray-100 transition-colors duration-200">
          <span className="text-lg font-medium text-gray-800">Todo 1</span>
          <button className="text-red-500 hover:text-red-600 transition-colors duration-200">
            Delete
          </button>
        </li>
        <li className="flex items-center justify-between px-6 py-4 border-b border-gray-200 hover:bg-gray-100 transition-colors duration-200 bg-light-green-200">
          <span className="text-lg font-medium text-gray-800">Todo 2</span>
          <button className="text-red-500 hover:text-red-600 transition-colors duration-200">
            Delete
          </button>
        </li>
      </ul>
    </main>
  );
}
```

<!-- client side rendering and server side -->

### การเรนเดอร์ข้อมูลด้วย Client Side Rendering และ Server Side Rendering

- ใน Next.js มีวิธีการเรนเดอร์ข้อมูลที่แตกต่างกันออกไป โดยมี Client Side Rendering (CSR) และ Server Side Rendering (SSR) ซึ่ง CSR จะทำการเรนเดอร์ข้อมูลบนเบราว์เซอร์ของผู้ใช้งาน ในขณะที่ SSR จะทำการเรนเดอร์ข้อมูลบนเซิร์ฟเวอร์ก่อนจะส่งข้อมูลไปยังเบราว์เซอร์ของผู้ใช้งาน

<!-- use client -->

- ใน Next.js จะถูกกำหนดให้เป็น server side rendering โดยค่าเริ่มต้น แต่ถ้าต้องการใช้ client side rendering ให้ใส่ 'use client' ในด้านบนของไฟล์ที่ต้องการใช้ client side rendering

```typescript
// src/app/index.tsx
'use client';

export default function Home() {
  ...
}
```

ทำไมต้องใช้ use client ในการเรนเดอร์ข้อมูลด้วย client side เพราะเราต้องการให้ข้อมูลถูกเรนเดอร์บนเบราว์เซอร์ของผู้ใช้งาน และเรียกใช้ข้อมูลจาก API ที่เราสร้างขึ้น

<!-- การเรียกใช้ API ใน Next.js -->

### การเรียกใช้ API ใน Next.js

```typescript
// src/app/index.tsx
export default function Home() {
   // กำหนด URL ของ API ที่ได้จากการสร้าง Mock API ใน MockAPI.io
  const api_url = "https://66c1e58bf83fffcb587a8c03.mockapi.io/todo";
  ...
}
```

- ใน Next.js สามารถใช้ `fetch` ในการเรียกใช้ API ได้โดยตรง ในตัวอย่างนี้เราจะใช้ `fetch` ในการเรียกใช้ API ที่เราสร้างขึ้น

```typescript
// src/app/index.tsx
export default function Home() {
  const api_url = "https://66c1e58bf83fffcb587a8c03.mockapi.io/todo";

  // สร้างฟังก์ชันเพื่อเรียกใช้ API
  const fetchTodos = async () => {
    const response = await fetch(api_url);
    const data = await response.json();
    console.log(data);
  };

  // เรียกใช้ฟังก์ชัน fetchTodos เมื่อโหลดหน้าเว็บ
  useEffect(() => {
    fetchTodos();
  }, []);

  ...
}
```

<!-- useEfect คืออะไร -->

### ความรู้เพิ่มเติม: useEffect คืออะไร?

- `useEffect` เป็นฟังก์ชันที่ใช้ในการเรียกใช้ฟังก์ชันหรือเอฟเฟกต์อื่นๆ ใน Component ของ React โดยมีรูปแบบการใช้งานดังนี้

```typescript
useEffect(() => {
  // โค้ดที่ต้องการให้ทำงาน
}, [dependencies]);
```

- ใน `useEffect` จะมีฟังก์ชันที่ต้องการให้ทำงานอยู่ภายในบล็อกแรก และ dependencies คือตัวแปรที่ถูกใช้ในฟังก์ชันที่อยู่ภายในบล็อกแรก ถ้า dependencies มีการเปลี่ยนแปลง `useEffect` จะทำงานอีกครั้ง

<!-- การแสดงข้อมูลที่ได้จาก API -->

### การแสดงข้อมูลที่ได้จาก API

- ในตัวอย่างนี้เราจะแสดงข้อมูลที่ได้จาก API ในรูปแบบของรายการ Todo โดยใช้ `map` ในการวนลูปข้อมูลที่ได้จาก API

```typescript
// src/app/index.tsx
export default function Home() {
  const api_url = "https://66c1e58bf83fffcb587a8c03.mockapi.io/todo";
  const [todos, setTodos] = useState<Todo[]>([]);

  const fetchTodos = async () => {
    const response = await fetch(api_url);
    const data = await response.json();
    setTodos(data);
  };

  // เรียกใช้ฟังก์ชัน fetchTodos เมื่อโหลดหน้าเว็บ
  useEffect(() => {
    fetchTodos();
  }, []);

  return (
    <main>
      <form className="flex items-center justify-center mt-8">...</form>
      <ul className="mt-8 bg-white shadow-lg rounded-lg w-3/4 mx-auto">
        {todos.map((todo) => (
          <li
            key={todo.id}
            className="flex items-center justify-between px-6 py-4 border-b border-gray-200 hover:bg-gray-100 transition-colors duration-200"
          >
            <span className="text-lg font-medium text-gray-800">
              {todo.title}
            </span>
            <button className="text-red-500 hover:text-red-600 transition-colors duration-200">
              Delete
            </button>
          </li>
        ))}
      </ul>
    </main>
  );
}
```

- ในตัวอย่างนี้เราใช้ `useState` ในการเก็บข้อมูลที่ได้จาก API และใช้ `map` ในการวนลูปข้อมูลที่ได้จาก API และแสดงผลในรูปแบบของรายการ Todo

<!-- การเพิ่ม Todo -->

### การเพิ่ม Todo

- ในตัวอย่างนี้เราจะเพิ่มฟังก์ชัน `addTodo` เพื่อเพิ่ม Todo โดยใช้ `fetch` ในการส่งข้อมูลไปยัง API

```typescript
// src/app/index.tsx

export default function Home() {
  const api_url = "https://66c1e58bf83fffcb587a8c03.mockapi.io/todo";
  const [todos, setTodos] = useState<Todo[]>([]);
  const [newTodo, setNewTodo] = useState<string>("");

  const fetchTodos = async () => {
    const response = await fetch(api_url);
    const data = await response.json();
    setTodos(data);
  };

  const addTodo = async (e: FormEvent) => {
    e.preventDefault();
    const response = await fetch(api_url, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        title: newTodo,
        completed: false,
      }),
    });
    const data = await response.json();
    setTodos([...todos, data]);
    setNewTodo("");
  };

  useEffect(() => {
    fetchTodos();
  }, []);

  return (
    <main>
      <form
        className="flex items-center justify-center mt-8"
        onSubmit={addTodo}
      >
        <input
          type="text"
          className="w-64 px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500"
          placeholder="What needs to be done?"
          value={newTodo}
          onChange={(e) => setNewTodo(e.target.value)}
        />
        <button
          type="submit"
          className="ml-4 px-4 py-2 bg-indigo-600 text-white rounded-md shadow-sm hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2"
        >
          Add
        </button>
      </form>
      <ul className="mt-8 bg-white shadow-lg rounded-lg w-3/4 mx-auto">
        {todos.map((todo) => (
          <li
            key={todo.id}
            className="flex items-center justify-between px-6 py-4 border-b border-gray-200 hover:bg-gray-100 transition-colors duration-200"
          >
            <span className="text-lg font-medium text-gray-800">
              {todo.title}
            </span>
            <button className="text-red-500 hover:text-red-600 transition-colors duration-200">
              Delete
            </button>
          </li>
        ))}
      </ul>
    </main>
  );
}
```

- ในตัวอย่างนี้เราใช้ `useState` ในการเก็บข้อมูลที่ผู้ใช้งานกรอกเข้ามา และใช้ `fetch` ในการส่งข้อมูลไปยัง API เมื่อผู้ใช้งานกดปุ่ม Add

<!-- การลบ Todo -->

### การลบ Todo

- ในตัวอย่างนี้เราจะเพิ่มฟังก์ชัน `deleteTodo` เพื่อลบ Todo โดยใช้ `fetch` ในการส่งข้อมูลไปยัง API

```typescript
// src/app/index.tsx
"use client";

export default function Home() {
  const api_url = "https://66c1e58bf83fffcb587a8c03.mockapi.io/todo";
  const [todos, setTodos] = useState<Todo[]>([]);
  const [newTodo, setNewTodo] = useState<string>("");

  const fetchTodos = async () => {
    const response = await fetch(api_url);
    const data = await response.json();
    setTodos(data);
  };

  const addTodo = async (e: FormEvent) => {
    e.preventDefault();
    const response = await fetch(api_url, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        title: newTodo,
        completed: false,
      }),
    });
    const data = await response.json();
    setTodos([...todos, data]);
    setNewTodo("");
  };

  const deleteTodo = async (id: string) => {
    await fetch(`${api_url}/${id}`, {
      method: "DELETE",
    });
    setTodos(todos.filter((todo) => todo.id !== id));
  };

  useEffect(() => {
    fetchTodos();
  }, []);

  return (
    <main>
      <form
        className="flex items-center justify-center mt-8"
        onSubmit={addTodo}
      >
        ...
      </form>
      <ul className="mt-8 bg-white shadow-lg rounded-lg w-3/4 mx-auto">
        {todos.map((todo) => (
          <li
            key={todo.id}
            className="flex items-center justify-between px-6 py-4 border-b border-gray-200 hover:bg-gray-100 transition-colors duration-200"
          >
            <span className="text-lg font-medium text-gray-800">
              {todo.title}
            </span>
            <button
              className="text-red-500 hover:text-red-600 transition-colors duration-200"
              onClick={() => deleteTodo(todo.id)}
            >
              Delete
            </button>
          </li>
        ))}
      </ul>
    </main>
  );
}
```

- ในตัวอย่างนี้เราใช้ `fetch` ในการส่งข้อมูลไปยัง API เมื่อผู้ใช้งานกดปุ่ม Delete

<!-- สรุป -->

## สรุป

ในคลาสนี้เราได้เรียนรู้เกี่ยวกับ Next.js และการใช้งาน API และการดำเนินการ CRUD พื้นฐาน โดยใช้ mock API จากบริการภายนอก โดยเราได้ทำการสร้างแอพพลิเคชั่น Todo ที่สามารถเพิ่ม Todo, แสดงรายการ Todo และลบ Todo ได้

## การเรียนรู้เพิ่มเติม

- [Next.js Documentation](https://nextjs.org/docs)
- [React Documentation](https://reactjs.org/docs/getting-started.html)
- [JavaScript Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
<!-- tailwind css -->
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)

## ลองทำ

- ลองเพิ่มฟังก์ชันแก้ไข Todo โดยใช้ `fetch` ในการส่งข้อมูลไปยัง API

<!-- ขอยคุณ -->

## ขอบคุณ

ขอบคุณที่ติดตามคลาสนี้จากเริ่มจนจบ หวังว่าคุณจะได้รับความรู้และประสบการณ์ที่ดีจากคลาสนี้

## License

This project is open source and available under the [MIT License](LICENSE).

<!-- /[]: # (README.md) -->
