TypeScript-এ interface এবং type alias—দুটোই object-এর structure (shape) define করতে সাহায্য করে। কিন্তু এগুলোর মধ্যে কিছু গুরুত্বপূর্ণ পার্থক্য আছে।

1. Interface শুধু “Object Structure” বর্ণনা করতে ব্যবহৃত হয়, কিন্তু Type অনেক কিছুর জন্য ব্যবহার হয়,
Interface কী define করতে পারে?

Object
Class structure
Function signature


Type alias কী define করতে পারে?

Primitive types (string, number)
Union
Tuple
Function
Object


উদাহরণ:
Interface (object structure)
interface User {
name: string;
age: number;
}

Type (object  )আরও অনেক কিছু
type User = {
name: string;
age: number;
};

type ID = string | number; // union
type Status = "active" | "inactive"; 
type Coordinate = [number, number]; 

 2. Interface extend করা সহজ, কিন্তু Type extend করা সীমাবদ্ধ
Interface easily extend করা যায়:
interface Person {
name: string;
}

interface Employee extends Person {
salary: number;
}

 Type-ও extend করা যায়, কিন্তু syntax আলাদা:
type Person = {
name: string;
};

type Employee = Person & {
salary: number;
};


3. Interface আবার declare করা যায় (merge হয়), কিন্তু Type never merge হয়
 একাধিক interface একই নামে লিখলে তারা merge হয়ে যায়।

Interface Merge Example:
interface User {
name: string;
}

interface User {
age: number;
}

const u: User = {
name: "Arif",
age: 22
};


4. Interface মূলত “Contracts” তৈরি করতে ব্যবহৃত হয়, Class structure enforce করতে পারে
Interface class implement করতে পারে:
interface Car {
brand: string;
drive(): void;
}

class Toyota implements Car {
brand = "Toyota";

drive() {
console.log("Driving");
}
}



5. Complex type composition-এ Type বেশি শক্তিশালী
যখন union, intersection, conditional types, utility types দরকার হয় → তখন type বেশি powerful।

Example (possible only with type):
type ApiResponse<T> = {
data: T;
success: boolean;
};

Interface এ conditional type করা যায় না।


6. Interface object-oriented pattern-এ ভালো মানায়, Type functional pattern-এ ভালো
Interface → class based architecture
Type → modern TS functional coding style

7. Performance (TypeScript compiler) দিক থেকে Interface একটু faster

Interface গুলো compile-time এ TypeScript সহজে optimize করতে পারে।
Type খুব flexible হওয়ায় কিছু ক্ষেত্রে slow হয় (tiny difference).

------------------------------------------

any, unknown, এবং never

TypeScript-এ এই তিনটি টাইপ একই রকম মনে হলেও, আসলে এদের চরিত্র আলাদা এবং কাজও আলাদা।

1. any — যেকোনো কিছু হতে পারে
any মানে হলো: “আমি কিছুই ধরে রাখছি না, তুমি যা খুশি কর।”
TypeScript এই টাইপে কোনো চেক করে না।
ভুল করলেও TypeScript কিছু বলবে না।
এটা ব্যবহার করলে কোড JavaScript-এর মতো আচরণ করবে।

উদাহরণ:
let data: any;

data = 10;      
data = "Hi";    
data = true;    

data.toUpperCase(); // ঠিকই চলবে, ভুল হলেও error দেবে না

চাইলে ভুল কোডও লিখতে পারো, তবুও TypeScript বাধা দেবে না।

2. unknown — যেকোনো কিছু হতে পারে, কিন্তু ব্যবহার করার আগে প্রমাণ দিতে হবে
unknown মানে: “আমিও যেকোনো কিছু হতে পারি, কিন্তু আমাকে ব্যবহার করার আগে তুমি আমাকে চেক করবে।”
TypeScript এতে টাইপ চেক করতে বাধ্য করে।
without checking, কোড চালাতে দেবে না।

উদাহরণ:
let value: unknown = "Hello";

// value.toUpperCase();  

if (typeof value === "string") {
  console.log(value.toUpperCase());  
}

unknown নিরাপদ (safe), কারণ TypeScript ভুল ব্যবহার আটকায়।

3. never — এমন কিছু যা কখনও ঘটে না
never মানে: “এই জিনিসটি কখনও রিটার্ন করবে না, বা কখনও ঘটবেই না।”
এই টাইপ সাধারণত ৩ জায়গায় দেখা যায়—

(a) ফাংশন যেগুলো কখনো শেষ হয় না
function infiniteLoop(): never {
  while (true) {}
}

(b) ফাংশন যেগুলো সবসময় error ছোড়ে
function throwError(): never {
  throw new Error("Something went wrong");
}

(c) এমন শর্ত যা কখনো সত্য হতে পারে না
type A = "x" | "y";

function check(value: A) {
  if (value === "x") {
  } else if (value === "y") {
  } else {
    const neverValue: never = value;
  }
}

never মানে অসম্ভব। এই জিনিস কখনো ঘটবে না।
