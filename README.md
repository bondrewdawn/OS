# 2022 Daily schedule of OS Tranining Camp

## Timeline

*November*

| Mon               | Tues              | Wed                          | Thur                         | Fri                          | Sat               | Sun               |
| ----------------- | ----------------- | ---------------------------- | ---------------------------- | ---------------------------- | ----------------- | ----------------- |
|                   | 1 <br> ([D1](#day-1-2022111)) | 2 <br> ([D2](#day-2-2022112)) | 3 <br> ([D3](#day-3-2022113)) | 4 <br> ([D4](#day-4-2022114)) | 5 <br> ([D5](#day-5-2022115)) | 6 <br> ([D6](#day-6-2022116)) |
|7 <br> ([D11](#day-7-2022117)) | 8 <br> ([D8](#day-8-2022118))       | 9 <br> ([D9](#day-9-2022119))            | 10 <br> ([D10](#day-10-20221110))         | 11  <br>  ([D11](#day-11-20221111))             | 12      <br>    ([D12](#day-12-20221112))       | 13    <br>    ([D13](#day-13-20221113))             |
|14         <br>    ([D14](#day-14-20221114))        | 15        <br>    ([D15](#day-15-20221115))                    | 16    <br>     ([D16](#day-16-20221116))                      | 17    <br>      ([D17](#day-17-20221117))       |18    <br>    ([D18](#day-18-20221118))            | 19   <br>     ([D19](#day-19-20221119))            | 20   <br>    ([D20](#day-20-20221120))            |
|21       <br>    ([D21](#day-21-20221121))         | 22     <br>    ([D22](#day-22-20221122))                         | 23     <br>    ([D23](#day-23-20221123))                         | 24    <br>    ([D24](#day-24-20221124))                        | 25      <br>    ([D25](#day-25-20221125))             | 26         <br>    ([D26](#day-26-20221126))           | 27         <br>    ([D27](#day-27-20221127))           |
|28       <br>    ([D28](#day-28-20221128))           | 29         <br>    ([D29](#day-29-2022729))                    | 30        <br>    ([D30](#day-30-20221130))                     | 

------

## Day 1 2022/11/1

### OS Tranining Camp

今天听了老师们的报告，感触颇多，之前学习过一段时间的Rust，同时又想对OS有较为深入的了解，包括riscv、qemu等知识，遂决定报名参加秋冬季OS训练营。

希望通过本次的训练营，在前辈们的帮助下，能够对Rust和OS有更进一步的认识。虽然去年就入坑了Rust，当时是啃的官方社区的TRPL，虽然过程很艰难，但是rust的配置实在是比c++配置起来好太多了，之后又看了rust官方社区的一些文档，但总是感觉缺少一点实践。因此从OS的项目做起，希望能够对Rust又更深入的理解。

首先从Rustlings开始学习，计划在一周之内完成rustling。与此同时，在WSL2中配置相应的实验环境。

## Day 2 2022/11/2

### OS Training Camp

今天完成了Rust实验环境的配置。于此同时，开始完成rustlings的实验，目前已经完成1/3，在Rust的移动语义部分还是较为卡壳，因此还需要对Rust的move semantic进行进一步的学习以加深对移动语义的理解。

#### Notes:

在quiz1中，我们实现了一个最为基础的计算价格的函数，但是注意该函数只能返回类型为`i32`的值，因此不能用于处理较复杂的情形，比如当输入值不合法（例如负数），后续会学到利用`Result`来处理结果。

在primitive_types中，注意只有`char`类型的变量才能够使用`is_numeric()`和`is_alphabetical()` API.
具体可以参考[link](https://doc.rust-lang.org/std/primitive.char.html#method.is_numeric)

`vector` initialization:

```rust
let a = vec![1, 2, 3, 4]; // create vector with 4 values
let b = vec![1; 100]; // in Rust, value first, create a vector with size of 100 and all values equal to 1
let c: Vec<i32> = Vec::new(); // this is also a way to creat the vector

fn zeros(size: u32) -> Vec<i32> {
    let mut zero_vec: Vec<i32> = Vec::with_capacity(size as usize); // this is another way to init vec
    for i in 0..size {
        zero_vec.push(0);
    }
    zero_vec
}
```

在Rust中，有Vec和slice类型，在上例的vector a中，可以通过`&a[1..=3]`取出其中特定的切片

```rust
let a = vec![1, 2, 3, 4, 5];
let new_slice = &a[1..=3];
assert_eq!([2, 3, 4], new_slice);
```

在vec2中，有两种方法批量修改vec的值：

```rust
fn vec_loop(mut v: Vec<i32>) -> Vec<i32> {
    for in in v.iter_mut() {
        *i *= 2;
    }
}

fn vec_map(v: &Vec<i32>) -> Vec<i32> {
    v.iter().map(|num| {
        num * 2 // closure of this
    }).collect()
}
```

在Rust中，对`string`进行操作，`trim()`用于删除空格，类似于Python中的`strip()` [link to strings3](./rustlings/exercises/strings/strings3.rs)

```rust
fn trim_me(input: &str) -> String {
    input.trim().to_string()
}

fn compose_me(input: &str) -> String {
    input.to_string() + " world!"
}

fn replace_me(input: &str) -> String {
    input.replace("cars", "balloons")
}

// there is also another way to replace the string, that is, use regex
use regex::Regex;
fn replace_regex(input: &str) -> String {
    let re = Regex::new(r"[cars]").unwrap();
    let res = re.replace_all(input, "balloons");
    res
}
```

Rust中，String和string_slice傻傻分不清：

```rust
fn string_slice(arg: &str) {
    println!("{}", arg);
}
fn string(arg: String) {
    println!("{}", arg);
}

fn main() {
    string_slice("blue");
    string("red".to_string());
    string("red".into());
    string("red".to_owned()); // 需要注意的是，在类型转换时，最好使用`to_owned()`,仅仅分配一个buffer。
    string(String::from("hi"));
    string("rust is fun!".to_owned());
    string("nice weather".into());
    string(format!("Interpolation {}", "Station"));
    string_slice(&String::from("abc")[0..1]);
    string_slice(" hello there ".trim());
    string("Happy Monday!".to_string().replace("Mon", "Fri"));
    string("mY sHiFt KeY iS sTicKY".to_lowercase());
}
```

需要注意的是，在类型转换时，最好使用`to_owned()`,仅仅分配一个buffer。[参考](https://users.rust-lang.org/t/to-string-vs-to-owned-for-string-literals/1441/5#:~:text=I%20now%20strongly%20prefer%20to_owned()%20for%20string%20literals%20over%20either%20of%20to_string()%20or%20into().)

## Day 3 2022/11/3

### OS Training Camp

今日进度：今天完成了Rustlings的enums，structs，options等小节.

`HashMap` in Rust. [link to hashmap](./rustlings/exercises/hashmaps/hashmaps1.rs)

```rust
let map: HashMap<String, i32> = HashMap::new();
map.insert(String::from("apple"), 3);
map.insert(String::from("mongo"), 2);


// remember the following
for fruit in fruit_kinds {
    if !map.contains_key(&fruit) {
        map.insert(fruit, 11);   // [link to hashmap2]
    }
}
```

上述代码段中的第二部分。[link to hashmap2](./rustlings/exercises/hashmaps/hashmaps2.rs)

I don't know how to implement HashMap3, so just reference to reddit.[[link to hashmap3](./rustlings/exercises/hashmaps/hashmaps3.rs)] [[reddit refer](https://www.reddit.com/r/rust/comments/w4vf5a/idiomatic_hashmap_access_from_rustlings_exercise/)] [[stackoverflow refer](https://stackoverflow.com/questions/73294897/correct-answer-at-rustlings-course-but-not-happy-with-it)]

在Rust中，当处理Options时，为了方便得到Ok()中的值，通常使用`unwrap()`来获得。此处对unwrap方法存疑。

[link to options3](./rustlings/exercises/options/options3.rs)中，完全依靠编译器编程。

Rust中的errors handling较难。

```rust
fn main() -> Result<(), Box<dyn error::error>> {
    let pretend_user_input = "32";
    let x: i32 = pretend_user_input.parse()?;
    println!("output={:?}", PositiveNonzeroInteger::new(x)?);
    Ok(())
}
```

There are two different possible `Result` types produced within `main()`, which are propagated using `?` operators. [? mark](https://doc.rust-lang.org/stable/rust-by-example/error/multiple_error_types/reenter_question_mark.html)

Under the hood, the `?` operator calls `From::from` on the error value to convert it to a boxed trait object, a `Box<dyn error::Error>`. This boxed trait object is polymorphic, and since all errors implement the `error::Error` trait, we can capture lots of different errors in one "Box" object. [boxing errors](https://doc.rust-lang.org/stable/rust-by-example/error/multiple_error_types/boxing_errors.html)

## Day 4 2022/11/4

### Rustlings

今天需要完成rustlings中的所有练习题，与此同时，截止到5号，完成rustlings的所有总结。
