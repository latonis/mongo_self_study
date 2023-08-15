```

db.sales.find($elemMatch:{{"storeLocation":"Online"},{"couponUsed": true}});


db.sales.find({
  items: {
    $elemMatch: { name: "laptop", price: { $gt: 800 }, quantity: { $gte: 1 } },
  },
})
```