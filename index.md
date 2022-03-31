---

theme : "night"
transition: "slide"
highlightTheme: "monokai"
# logoImg: "logo.png"
slideNumber: true
title: "JavaScript Testing"

---

# JavaScript Testing

<style>
:root {
    --r-heading1-size: 72px;
    --r-heading2-size: 48px;
}
pre {
  background: #303030;
  padding: 10px 16px;
  border-radius: 0.3em;
  counter-reset: line;
}
pre code[class*="="] .line {
  display: block;
  line-height: 1.8rem;
  font-size: 1em;
}
pre code[class*="="] .line:before {
  counter-increment: line;
  content: counter(line);
  display: inline-block;
  border-right: 3px solid #6ce26c;
  padding: 0 .5em;
  margin-right: .5em;
  color: #afafaf;
  width: 24px;
  text-align: right;
}

.reveal .slides > section > section {
  text-align:left; 
}

h1,h2,h3,h4 {
  text-align: center
}

p {
  text-align: center;
}

.present img {
    max-height: 65vh;
}
</style>

<script>

    window.onload = () => setTimeout(setLineNumber, 10)
    
    const setLineNumber = () => {
        const codeBlocks = document.querySelectorAll('pre code[class*="="]')
        codeBlocks.forEach(code => {
            const addLineSpan = code.innerHTML.trim().replaceAll('\n','</span><span class="line">')
            code.innerHTML = `<span class="line">${addLineSpan}</span>`    
        })
    }
    
</script>

---

# Yubin, Hsu

## NTAD / TSID

---

# Why test

--

Check

Quality

Confidence

Automation

--

How

---

# Golden Rule

--

design a test case to be 

<div style="padding: 0 100px;">

- simple
- short
- abstraction-free
- flat
- delightful to work with

</div>

---

# The test

--

## test name

1. What is being tested?
2. What scenario?
3. What is the expected result?

--

```typescript=
// 1. unit under test
describe('Products Service', function() {
  describe('Add new product', function() {
    // 2. scenario and 3. expectation
    it('When no price is specified, then the product status is pending approval', ()=> {
      const newProduct = new ProductService().add(...);
      expect(newProduct.status).to.equal('pendingApproval');
    });
  });
});
```

--

## AAA pattern

--

Arrange

Act

Assert

--

```typescript=
describe("Customer classifier", () => {
  test("When customer spent more than 500$, should be classified as premium", () => {
    //Arrange
    const customerToClassify = { spent: 505, joined: new Date(), id: 1 };
    const DBStub = sinon.stub(dataAccess, "getCustomer").reply({ id: 1, classification: "regular" });

    //Act
    const receivedClassification = customerClassifier.classifyCustomer(customerToClassify);

    //Assert
    expect(receivedClassification).toMatch("premium");
  });
});
```

--

## Use product language

--

code the expectation in a human-like language

```typescript=
expect(myValue > 5).toBe(true)
expect(myValue).toBeGreaterThan(5) // better to read
```

https://jestjs.io/docs/expect#expectextendmatchers

--

## Test public methods

--

## Don't "foo", use real input data

--

## Make each test case independent

~~global test fixture~~

--

## No catch, expect them

```typescript=
it(' should return 200 status code when call /api/todos API', async() => {
    try {
        const response = await axios.get('/api/todos')
        expect(response.status).toBe(200)
    } catch(error) {
        fail(`${error}`)
    }
})
```

↓

```typescript=
it(' should return 200 status code when call /api/todos API', async() => {
    const response = await axios.get('/api/todos')
    expect(response.status).toBe(200)
})
```

---

# Types of tests

- End-to-End, simulate user behavior
- Integration, multiple units work together
- Unit, simgle function/component
- Static, code typo or error

---

# TestPyramid

![](img/2022-03-31-23-47-28.png)

https://martinfowler.com/bliki/TestPyramid.html

--

---

end