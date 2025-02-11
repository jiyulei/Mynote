#graphql #query

``` graphQL
// get a company
{
	company(id: "1") {
		id
		name
		description
	}
}

// get multiple companies need to assign a key
{
	apple: company(id:"1") {
		id
		name
		description
	}
	google: company(id:"2") {
		id
		name
		description
	}
}
//上例会重复 避免重复可以定义fragment
fragment companyDetails on company {
	id
	name
	description
}
// 定义完后可以这样写：
{
	apple: company(id:"1") {
		...companyDetails
	}
	google: company(id:"2") {
		...companyDetails
	}
}
```