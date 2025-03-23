# SOLID Design Principles (বাংলায়)

SOLID হল পাঁচটি গুরুত্বপূর্ণ Object-Oriented Design (OOD) নীতি যা সফটওয়্যার ডিজাইনকে আরও মডুলার, রিফ্যাক্টর-যোগ্য ও সহজে পরিবর্তনযোগ্য করে।

---

## 1️⃣ S - Single Responsibility Principle (SRP)

### **নিয়ম:**
একটি ক্লাস কেবলমাত্র **একটি কাজ করবে এবং একটাই কারণের জন্য পরিবর্তিত হবে।**

### **ব্যাখ্যা:**
এই নীতির মূল কথা হলো, **একটি ক্লাস শুধুমাত্র একটি কাজের জন্য দায়ী হবে।**
যদি কোনো ক্লাস একাধিক কাজ করে, তাহলে ভবিষ্যতে পরিবর্তন করলে সবকিছুতে প্রভাব ফেলতে পারে।

### 🎯 **বাস্তব উদাহরণ:**
📌 **একজন শিক্ষক ও রেজাল্ট প্রসেসিং**
একজন শিক্ষক যদি একসঙ্গে পড়ানো, পরীক্ষার প্রশ্ন তৈরি করা, এবং রেজাল্ট প্রসেসিংয়ের কাজ করেন, তাহলে এটি একটি খারাপ ডিজাইন। বরং শিক্ষক শুধুমাত্র পড়ানোর কাজ করবেন এবং রেজাল্ট প্রসেসিং অন্য কেউ করবে।

### **ভুল উদাহরণ (Bad Design)**
```php
class ReportCard {
    public function generateResult() {
        // রেজাল্ট তৈরি করা
    }
    public function printReport() {
        // রিপোর্ট প্রিন্ট করা
    }
}
```
এখানে **ReportCard** ক্লাস দুটি আলাদা কাজ করছে – রেজাল্ট তৈরি করা ও রিপোর্ট প্রিন্ট করা।

### ✅ **ভালো উদাহরণ (SRP অনুসারে)**
```php
class ResultGenerator {
    public function generateResult() {
        // রেজাল্ট তৈরি করা
    }
}

class ReportPrinter {
    public function printReport() {
        // রিপোর্ট প্রিন্ট করা
    }
}
```
এখন **ResultGenerator** কেবলমাত্র রেজাল্ট তৈরি করছে এবং **ReportPrinter** শুধু রিপোর্ট প্রিন্ট করছে। এতে **SRP মানা হয়েছে।**

---

## 2️⃣ O - Open/Closed Principle (OCP)

### **নিয়ম:**
একটি ক্লাস বা মডিউল **প্রসারণযোগ্য (Extendable)** হওয়া উচিত, কিন্তু **পরিবর্তন (Modify)** করা উচিত নয়।

### **ব্যাখ্যা:**
কোনো ক্লাসের নতুন ফিচার যোগ করতে হলে তার মূল কোড পরিবর্তন না করে **এক্সটেনশন বা নতুন ক্লাস যোগ করা উচিত।**

### 🎯 **বাস্তব উদাহরণ:**
📌 **পেমেন্ট গেটওয়ে প্রসেসিং**
ধরা যাক, একটি ই-কমার্স সাইটে প্রথমে শুধুমাত্র **Bkash Payment** যুক্ত করা হলো। পরে **Nagad Payment** যোগ করতে চাইলে যদি মূল কোড পরিবর্তন করতে হয়, তবে এটি খারাপ ডিজাইন। বরং, নতুন ক্লাস তৈরি করলেই নতুন পেমেন্ট মেথড যোগ করা যাবে।

### ✅ **ভালো উদাহরণ (OCP অনুসারে)**
```php
interface PaymentGateway {
    public function pay();
}

class BkashPayment implements PaymentGateway {
    public function pay() {
        echo "Bkash payment processing...";
    }
}

class NagadPayment implements PaymentGateway {
    public function pay() {
        echo "Nagad payment processing...";
    }
}

class PaymentProcessor {
    public function process(PaymentGateway $gateway) {
        $gateway->pay();
    }
}
```
এখন নতুন গেটওয়ে যোগ করতে শুধু **PaymentGateway ইন্টারফেস ইমপ্লিমেন্ট করা নতুন ক্লাস তৈরি করলেই হবে।**

---

## 3️⃣ L - Liskov Substitution Principle (LSP)

### **নিয়ম:**
সাবক্লাস (Child Class) অবশ্যই তার প্যারেন্ট ক্লাসের (Parent Class) আচরণ পরিবর্তন না করেই প্রতিস্থাপন করতে পারবে।

### 🎯 **বাস্তব উদাহরণ:**
📌 **গাড়ি ও ইলেকট্রিক কার**
একটি **গাড়ি (Car)** যদি **ইলেকট্রিক গাড়ির (ElectricCar)** সাবক্লাস হয়, তাহলে ইলেকট্রিক গাড়ির **Refuel()** মেথড থাকা উচিত নয়, কারণ এটি পেট্রোল ব্যবহার করে না। বরং, **ChargeBattery()** নামে আলাদা মেথড থাকা উচিত।

### ✅ **ভালো উদাহরণ (LSP অনুসারে)**
```php
interface Shape {
    public function getArea();
}

class Rectangle implements Shape {
    protected $width, $height;

    public function __construct($w, $h) {
        $this->width = $w;
        $this->height = $h;
    }

    public function getArea() {
        return $this->width * $this->height;
    }
}

class Square implements Shape {
    protected $side;

    public function __construct($s) {
        $this->side = $s;
    }

    public function getArea() {
        return $this->side * $this->side;
    }
}
```
এখানে **Rectangle ও Square আলাদা Shape ইন্টারফেস ইমপ্লিমেন্ট করছে**, তাই কোনো সমস্যাই হবে না।

---

## 4️⃣ I - Interface Segregation Principle (ISP)

### **নিয়ম:**
একটি বড় ইন্টারফেসকে ছোট ছোট নির্দিষ্ট ইন্টারফেসে ভাগ করা উচিত, যাতে কোনো ক্লাস অপ্রয়োজনীয় মেথড ইমপ্লিমেন্ট করতে বাধ্য না হয়।

### 🎯 **বাস্তব উদাহরণ:**
📌 **প্রিন্টার মেশিনের ফিচার**
একটি **Basic Printer** শুধু প্রিন্ট করতে পারে, কিন্তু **All-in-One Printer** প্রিন্ট, স্ক্যান ও ফ্যাক্স করতে পারে। তাই, Basic Printer-কে অপ্রয়োজনীয় scan() বা fax() মেথড ইমপ্লিমেন্ট করতে বাধ্য করা উচিত নয়।

### ✅ **ভালো উদাহরণ (ISP অনুসারে)**
```php
interface Printer {
    public function print();
}

interface Scanner {
    public function scan();
}

interface Fax {
    public function fax();
}

class BasicPrinter implements Printer {
    public function print() {
        echo "Printing...";
    }
}
```
---

## 5️⃣ D - Dependency Inversion Principle (DIP)

### **নিয়ম:**
উচ্চ স্তরের মডিউল (High-Level Module) নিম্ন স্তরের মডিউলের (Low-Level Module) উপর নির্ভর করবে না; বরং তারা একটি অ্যাবস্ট্রাকশন (Interface) এর উপর নির্ভর করবে।

### 🎯 **বাস্তব উদাহরণ:**
📌 **একটি সুইচ যেকোনো ডিভাইস অন করতে পারবে**
একটি সাধারণ **Switch** শুধুমাত্র Light অন করতে পারবে না, এটি **Fan** বা **TV**-ও অন করতে পারবে। তাই, Switch ক্লাসকে নির্দিষ্ট Light বা Fan-এর উপর নির্ভর করা উচিত নয়।

### ✅ **ভালো উদাহরণ (DIP অনুসারে)**
```php
interface Switchable {
    public function turnOn();
}

class Light implements Switchable {
    public function turnOn() {
        echo "Light is ON";
    }
}

class Fan implements Switchable {
    public function turnOn() {
        echo "Fan is ON";
    }
}
```

এখন **Switch** কোনো নির্দিষ্ট ডিভাইসের উপর নির্ভর না করে **Switchable ইন্টারফেসের উপর নির্ভর করছে।**
