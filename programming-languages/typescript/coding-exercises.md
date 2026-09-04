# Coding Exercises

## Q and A
### P1. Product Data Processing
API에서 다음과 같은 데이터가 반환된다고 가정하자. 다음 함수를 구현하라. 
```
function searchProducts(
  products: Product[],
  searchTerm: string,
  category?: string,
  maxPrice?: number
): Product[]
```

[API response data]
```
interface Product {
  id: number;
  name: string;
  category: string;
  price: number;
  available: boolean;
  stock: number;
}

const products: Product[] = [
  { id: 1, name: "Milk", category: "Dairy", price: 1.5, available: true, stock: 20 },
  { id: 2, name: "Cheese", category: "Dairy", price: 3.5, available: true, stock: 5 },
  { id: 3, name: "Apple", category: "Fruit", price: 2.0, available: true, stock: 30 },
  { id: 4, name: "Banana", category: "Fruit", price: 1.2, available: false, stock: 0 },
  { id: 5, name: "Bread", category: "Bakery", price: 1.8, available: true, stock: 10 }
];
```

다음 조건을 모두 만족해야 한다.
1. Search
searchTerm이 상품 이름에 포함된 상품을 반환한다.
검색은 case-insensitive해야 한다.
```
"milk" → Milk
"MILK" → Milk
"ilk"  → Milk
```


```
describe('product data processing tests', () => {
  it('should return true', () => {
    const result = true
    expect(result).toBe(true)
  })
  
  it('should return the data which is case-insensitive in name', () => {
    const searchTerm = 'milk'
    const result = searchProducts(products, searchTerm)

    expect(result.length).toBe(1)
  })
})
```

```
function searchProducts(products: Product[], searchTerm: string, category?: string, maxPrice?: number) {
  if (!products || products.length === 0) {
    throw new Error('Product is empty')
  }

  if (!searchTerm) {
    throw new Error('searchTerm is empty')
  }

  const normalizedTerm = searchTerm?.trim().toLowerCase(searchTerm)

  const filteredProducts = products.filter(product => product.name.toLowercase().includes(normalizedTerm))

  return filteredProducts
}
```
> 정확한 일치를 찾아야 하면,
> ```const filteredProducts = products.filter(product => product.name.toLowercase() === normalizedTerm)```

