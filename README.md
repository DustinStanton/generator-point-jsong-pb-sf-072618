
# Groups and the Generator Point

### Mathematical Groups

Unlike fields, groups have only a single operation. In our case, Point Addition is our operation. We also have a few other properties like closure, invertibility, commutativity and associativity. Lastly, we need the identity.

It turns out that we have all of these things with Point Addition. Let's look at each property

#### Identity

If you haven't guessed by now, the identity is defined as the point at infinity. This is the point, when added to any other point produces the other point. So:

0 + P = P

We call 0 the point at infinity because visually, it's the point that exists to help the math work out:

image::intersect2-1.png[Vertical Line]

#### Closure

This is perhaps the easiest to prove since we generated the group in the first place by adding G over and over. Thus, two different elements look like this:

`aG + bG`

We know that the result is going to be:

`(a+b)G`

How do we know if this element is in the group? If a+b < n, then we know it's in the group by definition. If a+b >= n, then we know a < n and b < n, so a+b<2n so a+b-n<n.

`(a+b-n)G=aG+bG-nG=aG+bG-O=aG+bG`

So we know that this element is in the group, proving closure.

#### Invertibility

Mathematically, we know that if aG is in the group, (n-a)G is also in the group. You can add them together to get 0.

#### Commutativity

P+Q=Q+P because they end up drawing the same line.

The equations for figuring out the third point also make this clear:

P~1~=(x~1~,y~1~), P~2~=(x~2~,y~2~), P~3~=(x~3~,y~3~)

x~3~=s^2^-x~1~-x~2~

y~3~=s(x~1~-x~3~)-y~1~=s(x~2~-x~3~)-y~2~

You can swap P~1~ and P~2~ to get the exact same equation.

#### Associativity

This is the hardest to prove but we'll dive deeper into it later.

### Try it


```python
from ecc import FieldElement, Point

prime = 223
a = FieldElement(0, prime)
b = FieldElement(7, prime)
G = Point(FieldElement(47, prime), FieldElement(71, prime), a, b)
inf = Point(None, None, a, b)
total = G
count = 1

while total != inf:
    print('{}:{}'.format(count, total))
    total += G
    count += 1
print('{}:{}'.format(count, total))
```

## Group order

We said that an elliptic curve defined over a finite field has a finite number of points. An important question that we need to answer is: how many points are there exactly?

The number of points in a group is called the order of the group. [[Source]](http://andrea.corbellini.name/2015/05/23/elliptic-curve-cryptography-finite-fields-and-discrete-logarithms/)

### Try it

#### Find out what the order of the group generated by (15, 86) is on  \\( y^2 = x^3 + 7: F_{223} \\)

#### Hint: add the point to itself until you get the point at infinity


```python
from ecc import FieldElement, Point

prime = 223
a = FieldElement(0, prime)
b = FieldElement(7, prime)

x = FieldElement(15, prime)
y = FieldElement(86, prime)
p = Point(x, y, a, b)
inf = Point(None, None, a, b)

# start product at point
# start counter at 1
# loop until you get point at infinity (0)
    # add the point to the product
    # increment counter
# print counter
```

### Test Driven Exercise

It's your turn to write a test now. Write the test below checking for the group order using the comments as a guide.


```python
from helper import run_test
from unittest import TestCase
from ecc import FieldElement, Point

class ECCTest(TestCase):

    def test_rmul(self):
        # tests the following scalar multiplications
        # 2*(192,105)
        # 2*(143,98)
        # 2*(47,71)
        # 4*(47,71)
        # 8*(47,71)
        # 21*(47,71)
        prime = 223
        a = FieldElement(0, prime)
        b = FieldElement(7, prime)

        multiplications = (
            # (coefficient, x1, y1, x2, y2)
            (2, 192, 105, 49, 71),
            (2, 143, 98, 64, 168),
            (2, 47, 71, 36, 111),
            (4, 47, 71, 194, 51),
            (8, 47, 71, 116, 55),
            (21, 47, 71, None, None),
        )

        # iterate over the multiplications
            # Initialize points this way:
            # x1 = FieldElement(x1_raw, prime)
            # y1 = FieldElement(y1_raw, prime)
            # p1 = Point(x1, y1, a, b)
            # initialize the second point based on whether it's the point at infinity
            # x2 = FieldElement(x2_raw, prime)
            # y2 = FieldElement(y2_raw, prime)
            # p2 = Point(x2, y2, a, b)
            # check that the product is equal to the expected point
        raise NotImplementedError
```

### Run your tests!

See whether your test works by running the cell below (remember to run the cell above too!).


```python
run_test(ECCTest('test_rmul'))
```

### Generator Point

You can think of the generator G as the first point after infinity on the curve. Begin with infinity and add G; the result is G. Add G to this and you get 2G. Add G to this and you get 3G. And so on. If you add G a total of n times (where n is the order of the curve) you will be back at infinity, where you started; the whole curve is a never-ending loop. The order n is how many distinct points are on the curve, or in Bitcoin terms, how many possible private keys there are (plus 1 for the point at infinity). [[Source]](https://bitcoin.stackexchange.com/a/34133)


```python
# Confirgming G is on the curve
p = 2**256 - 2**32 - 977
x = 0x79BE667EF9DCBBAC55A06295CE870B07029BFCDB2DCE28D959F2815B16F81798
y = 0x483ADA7726A3C4655DA4FBFC0E1108A8FD17B448A68554199C47D08FFB10D4B8
print(y**2 % p == (x**3 + 7) % p)
```


```python
# Confirming order of G is n
from ecc import G

n = 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEBAAEDCE6AF48A03BBFD25E8CD0364141
print(n*G)
```
