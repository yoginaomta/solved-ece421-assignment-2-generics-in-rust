Download Link: https://assignmentchef.com/product/solved-ece421-assignment-2-generics-in-rust
<br>
Generics is the topic of generalizing types and functionalities to broader cases. Generics is useful as it helps reduce duplicated code. However, using generics requires taking great care to specify over which types a generic type is actually considered valid. The simplest and most common use of generics is for type parameters. For example, defining a generic function named myFunction that takes an argument T of any type:<strong>  </strong>

fn myFunction&lt;T&gt;(arg: T) { … }

Although types in Rust are erased, generics are also reified. Consider the following code:

<table width="639">

 <tbody>

  <tr>

   <td width="639">struct Bag&lt;T&gt; {     t: T,}fn main() {let b = Bag { t: 42 }; }</td>

  </tr>

 </tbody>

</table>

A Bag&lt;u8&gt; is not the same type as a Bag&lt;u32&gt;. Although it implements the same methods, it has a different size.

<strong>Question 1: </strong>Rewrite the Bag struct to hold an array of 3 items of type T.

<strong>Question 2: </strong>Write a function named <em>BagSize</em> that takes a Bag as an input and returns the size of a Bag.

Test your function with the following code:

let b1 = Bag {items: [1u8, 2u8, 3u8], }; let b2 = Bag {items: [1u32, 2u32, 3u32], };

The code should give the following output:

size of First Bag = 3 bytes size of Second Bag = 12 bytes

<strong>Question 3: </strong>Rewrite the code for b1 and b2 without using any generics to produce the same output. [Hint: you will have to duplicate a lot of code]

However, monomorphization (https://en.wikipedia.org/wiki/Monomorphism) increases binary size, which is considered as its negative aspect. For example, if the Bag struct defined above has a lot of methods which uses a lot of different Ts, this will result in creating a lot of variants. Imagine if we have multiple generic types!




1




<strong>ECE 421 | Exploring Software Development Domains </strong>










<strong>Question 4:</strong> Consider the following code:

<table width="639">

 <tbody>

  <tr>

   <td width="639">let vec1 = vec![12, 32, 13]; let vec2 = vec![44, 55, 16]; { let vec1_iter = vec1.iter(); }{let vec_chained = vec1.iter().chain(vec2.iter());}{ let vec1_2=vec![vec1, vec2];let vec_flattened = vec1_2.iter().flatten(); }</td>

  </tr>

 </tbody>

</table>

Similar to Question2, report the sizes of vec1_iter, vec_chained, and vec_flattened.

<strong>Question 5:</strong> Run the code implemented in Question 4 but after boxing <a href="https://doc.rust-lang.org/std/boxed/struct.Box.html">(</a><a href="https://doc.rust-lang.org/std/boxed/struct.Box.html">https://doc.rust</a><a href="https://doc.rust-lang.org/std/boxed/struct.Box.html">–</a>

<a href="https://doc.rust-lang.org/std/boxed/struct.Box.html">lang.org/std/boxed/struct.Box.html</a><a href="https://doc.rust-lang.org/std/boxed/struct.Box.html">)</a> all the vectors. Again, report the sizes of vec1_iter, vec_chained, and vec_flattened.

<strong>Question 6:</strong> Explain the results obtained in Question 4 and 5 while illustrating the contents of both the stack and the heap in each case. [i.e., How does Rust model the iterators in each case?]

<strong>Question 7</strong>: What is polymorphism? Does Rust support polymorphism?

<h1>Working with the Compiler Explorer</h1>

Now, let’s explore one advantage of monomorphization. Since, in this case, the compiler knows the instantiated types ahead of time, this generates a lot of opportunities for optimizing code. Consider the following code:

<table width="639">

 <tbody>

  <tr>

   <td width="639">fn equal&lt;T&gt;(x: T, y: T) -&gt; bool where T: PartialEq,{x == y}fn compare(x: &amp;str, y: &amp;str) {   equal(x, y);equal(x.len(), y.len());}pub fn main() {let mut i = std::env::args();let (x, y) = (i.next().unwrap(), i.next().unwrap());     compare(&amp;x, &amp;y);}</td>

  </tr>

 </tbody>

</table>

<strong>Question 8:</strong> Analyze the code using the compiler explorer (<a href="https://godbolt.org/">https://godbolt.org/</a><a href="https://godbolt.org/">)</a> and report how many times the function <em>equal</em> has been called (Please mention the lines numbers).

<strong>Question 9:</strong> Now, use the optimization flag -O and recompile the code. How many times has the function equal been called? Explain what happened.


