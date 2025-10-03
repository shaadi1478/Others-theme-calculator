# Calculator App

## Description
এই একটি ওয়েব বেসড ক্যালকুলেটর। এটি HTML, Tailwind CSS, এবং JavaScript ব্যবহার করে তৈরি করা হয়েছে। এটি সংখ্যা এবং মৌলিক অপারেশনগুলো ( +, -, *, / ) সমর্থন করে।  

## Features
- সংখ্যা বোতাম (0-9) এবং `00`  
- অপারেটর বোতাম (+, -, *, /)  
- Clear (AC) এবং Delete (DE)  
- Decimal `.`  
- Keyboard support (Numbers, +, -, *, /, Enter, Backspace, Ctrl+C)  
- Responsive design (মোবাইল এবং ডেস্কটপ উভয়েই কাজ করে)  

## Installation
1. কোডটি ডাউনলোড করুন অথবা কপি করুন।  
2. ব্রাউজারে HTML ফাইল খুলুন।  

## Usage
- সংখ্যা বোতাম ক্লিক করুন অথবা কীবোর্ড ব্যবহার করুন।  
- `=` বোতাম বা Enter চাপুন হিসাবের জন্য।  
- `AC` সব মুছে দেয়, `DE` এক অক্ষর মুছে দেয়।  

## Technologies
- HTML5  
- Tailwind CSS  
- JavaScript

# JavaScript Code Explanation

## Variables

```js
const screen = document.getElementById('screen'); // স্ক্রিন DOM এলিমেন্ট
const keys = document.querySelector('section div.grid'); // বোতাম কনটেইনার
let expr = ''; // এক্সপ্রেশন স্টোর করার ভেরিয়েবল
let justEvaluated = false; // হিসাব শেষ হয়েছে কিনা চেক করার জন্য
```

## Render Function

```js
const render = () => {
  screen.textContent = expr.length ? expr : '0';
  // স্ক্রিন আপডেট করে, খালি হলে 0 দেখায়
};
```

## Append Function

```js
const append = (val) => {
  if(justEvaluated && /[0-9.]/.test(val)) expr = ''; // নতুন সংখ্যা এলে রিসেট
  justEvaluated = false;

  if(expr === '' && /[+\-*/]/.test(val)) return; // শুরুতে অপারেটর ব্লক
  if(/[+\-*/]$/.test(expr) && /[+\-*/]/.test(val)) return; // ডাবল অপারেটর ব্লক
  if(expr === '0' && /\d/.test(val) && val !== '.') expr = ''; // লিডিং জিরো রিমুভ
  if(val === '.') {
    const lastNum = expr.split(/[+\-*/]/).pop();
    if(lastNum.includes('.')) return; // ডাবল ডট ব্লক
  }

  expr += val; // এক্সপ্রেশনে মান যোগ
  render(); // স্ক্রিন আপডেট
};
```

## Clear & Delete

```js
const clearAll = () => { expr = ''; render(); }; // সব মুছে দেয়
const del = () => { expr = expr.length > 1 ? expr.slice(0, -1) : ''; render(); }; // শেষ অক্ষর মুছে দেয়
```

## Evaluate Function

```js
const evaluate = () => {
  try {
    if(!expr) return;
    if(/[+\-*/]$/.test(expr)) expr = expr.slice(0, -1); // শেষ অপারেটর রিমুভ
    if(!/^[-+*/.0-9\s]+$/.test(expr)) return; // ইনভ্যালিড চেক
    expr = String(eval(expr)); // হিসাব করা
    justEvaluated = true;
    render();
  } catch(e) {
    screen.textContent = 'Error'; expr = '';
  }
};
```

## Button Click Event

```js
keys.addEventListener('click', (e) => {
  const btn = e.target.closest('button');
  if(!btn) return;
  const val = btn.dataset.value;
  const action = btn.dataset.action;

  if(action === 'clear') return clearAll();
  if(action === 'delete') return del();
  if(action === 'equals') return evaluate();
  if(val) return append(val);
});
```

## Keyboard Support

```js
window.addEventListener('keydown', (e) => {
  const k = e.key;
  if(/^[0-9]$/.test(k)) return append(k); // সংখ্যা
  if(['.','+','-','*','/'].includes(k)) return append(k); // অপারেটর
  if(k==='Enter'||k==='='){ e.preventDefault(); return evaluate(); } // Enter বা =
  if(k==='Backspace') return del(); // Backspace
  if(k.toLowerCase()==='c' && (e.ctrlKey||e.metaKey)) return clearAll(); // Ctrl+C
});
```

## Initial Render

```js
render(); // প্রথমে স্ক্রিন আপডেট
```

* কীবোর্ড ও বোতাম দুইভাবেই ইনপুট নেওয়া যায়।
* সমস্ত অপারেশন ও ভ্যালিডেশন JS দিয়ে হ্যান্ডেল করা হয়েছে.
