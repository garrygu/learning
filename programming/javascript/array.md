

# Scenarios
## Remove the first element of an array
- list.shift()
- list.splice(1)
- const [, ...newList]= list
- const newList = list.filter((_,i)=>i)  //bad performance
- const newList = list.slice(1)  //slice extracts up to but not including end.
