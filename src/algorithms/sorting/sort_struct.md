## 结构体 Vector 排序

<!--
> [algorithms/sorting/sort_struct.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/algorithms/sorting/sort_struct.md)
> <br />
> commit - b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![std-badge]][std] [![cat-science-badge]][cat-science]

依据自然顺序（按名称和年龄），对具有 `name` 和 `age` 属性的 Person 结构体 Vector 排序。为了使 Person 可排序，你需要四个 traits：[`Eq`]、[`PartialEq`]、[`Ord`]，以及 [`PartialOrd`]。这些 traits 可以被简单地派生。你也可以使用 [`vec:sort_by`] 方法自定义比较函数，仅按照年龄排序。

```rust,edition2018
#[derive(Debug, Eq, Ord, PartialEq, PartialOrd)]
struct Person {
    name: String,
    age: u32
}

impl Person {
    pub fn new(name: String, age: u32) -> Self {
        Person {
            name,
            age
        }
    }
}

fn main() {
    let mut people = vec![
        Person::new("Zoe".to_string(), 25),
        Person::new("Al".to_string(), 60),
        Person::new("John".to_string(), 1),
    ];

    // Sort people by derived natural order (Name and age)
    people.sort();

    assert_eq!(
        people,
        vec![
            Person::new("Al".to_string(), 60),
            Person::new("John".to_string(), 1),
            Person::new("Zoe".to_string(), 25),
        ]);

    // Sort people by age
    people.sort_by(|a, b| b.age.cmp(&a.age));

    assert_eq!(
        people,
        vec![
            Person::new("Al".to_string(), 60),
            Person::new("Zoe".to_string(), 25),
            Person::new("John".to_string(), 1),
        ]);

}
```

[`Eq`]: https://doc.rust-lang.org/std/cmp/trait.Eq.html 
[`PartialEq`]: https://doc.rust-lang.org/std/cmp/trait.PartialEq.html
[`Ord`]: https://doc.rust-lang.org/std/cmp/trait.Ord.html
[`PartialOrd`]: https://doc.rust-lang.org/std/cmp/trait.PartialOrd.html
[`vec:sort_by`]: https://doc.rust-lang.org/std/vec/struct.Vec.html#method.sort_by
