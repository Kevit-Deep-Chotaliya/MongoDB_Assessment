# MongoDB Assessment

## 1.Create database and collection

**Query:**

```js
use intern_mongodb_assessment
db.createCollection("students")
```

**Output:**

```js
switched to db intern_mongodb_assessment
{ ok: 1 }
```

## 2. Insert multiple documents

**Query:**

```js
db.students.insertMany(
  [
  { studentId: 101, name: "Aarav", age: 19, course: "Physics", city: "Rajkot", marks: 78, skills: ["JavaScript", "MongoDB"], status: "active" },
  { studentId: 102, name: "Diya", age: 21, course: "Biology", city: "Ahmedabad", marks: 88, skills: ["Python", "MongoDB"], status: "active" },
  { studentId: 103, name: "Kabir", age: 18, course: "Physics", city: "Rajkot", marks: 65, skills: ["JavaScript", "HTML"], status: "inactive" },
  { studentId: 104, name: "Meera", age: 22, course: "Chemistry", city: "Surat", marks: 91, skills: ["Python", "SQL"], status: "active" },
  { studentId: 105, name: "Arjun", age: 20, course: "Biology", city: "Vadodara", marks: 74, skills: ["Java", "MongoDB"], status: "active" },
  { studentId: 106, name: "Riya", age: 23, course: "Chemistry", city: "Ahmedabad", marks: 84, skills: ["JavaScript", "SQL"], status: "inactive" },
  { studentId: 107, name: "Vivaan", age: 19, course: "Physics", city: "Surat", marks: 82, skills: ["Python", "MongoDB"], status: "active" },
  { studentId: 108, name: "Anaya", age: 24, course: "Biology", city: "Rajkot", marks: 95, skills: ["JavaScript", "Python"], status: "active" },
  { studentId: 109, name: "Ishaan", age: 21, course: "Chemistry", city: "Vadodara", marks: 69, skills: ["Java", "SQL"], status: "active" },
  { studentId: 110, name: "Myra", age: 20, course: "Physics", city: "Ahmedabad", marks: 87, skills: ["MongoDB", "SQL"], status: "inactive" }
]
)
```

**Output:**

```js
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('6abcde2fc4143b29cf7966a0'),
    '1': ObjectId('6abcde2fc4143b29cf7966a1'),
    '2': ObjectId('6abcde2fc4143b29cf7966a2'),
    '3': ObjectId('6abcde2fc4143b29cf7966a3'),
    '4': ObjectId('6abcde2fc4143b29cf7966a4'),
    '5': ObjectId('6abcde2fc4143b29cf7966a5'),
    '6': ObjectId('6abcde2fc4143b29cf7966a6'),
    '7': ObjectId('6abcde2fc4143b29cf7966a7'),
    '8': ObjectId('6abcde2fc4143b29cf7966a8'),
    '9': ObjectId('6abcde2fc4143b29cf7966a9')
  }
}
```

## 3. Find all students who are studying `Physics` and whose age is greater than or equal to 19. Return only `name`, `age`, `course`, and `marks` in the result. Do not display `_id`.

**Query:**

```js
db.students.find(
  { course: "Physics", age: { $gte: 19 } },
  { name: 1, age: 1, course: 1, marks: 1, _id: 0 }
)
```

**Output:**

```js
{
  name: 'Aarav',
  age: 19,
  course: 'Physics',
  marks: 78
}
{
  name: 'Vivaan',
  age: 19,
  course: 'Physics',
  marks: 82
}
{
  name: 'Myra',
  age: 20,
  course: 'Physics',
  marks: 87
}
```

## 4. Find all students whose `city` is either `Rajkot` or `Ahmedabad` and whose `marks` are greater than or equal to 80.Return the results sorted by `marks` from highest to lowest.

**Query:**

```js
db.students.find({
  "city": { "$in": ["Rajkot", "Ahmedabad"] },
  "marks": { "$gte": 80 }
}).sort({ "marks": -1 })
```

**Output:**

```js
{
  _id: ObjectId('6abcde2fc4143b29cf7966a7'),
  studentId: 108,
  name: 'Anaya',
  age: 24,
  course: 'Biology',
  city: 'Rajkot',
  marks: 95,
  skills: [
    'JavaScript',
    'Python'
  ],
  status: 'active'
}
{
  _id: ObjectId('6abcde2fc4143b29cf7966a1'),
  studentId: 102,
  name: 'Diya',
  age: 21,
  course: 'Biology',
  city: 'Ahmedabad',
  marks: 88,
  skills: [
    'Python',
    'MongoDB'
  ],
  status: 'active'
}
{
  _id: ObjectId('6abcde2fc4143b29cf7966a9'),
  studentId: 110,
  name: 'Myra',
  age: 20,
  course: 'Physics',
  city: 'Ahmedabad',
  marks: 87,
  skills: [
    'MongoDB',
    'SQL'
  ],
  status: 'inactive'
}
{
  _id: ObjectId('6abcde2fc4143b29cf7966a5'),
  studentId: 106,
  name: 'Riya',
  age: 23,
  course: 'Chemistry',
  city: 'Ahmedabad',
  marks: 84,
  skills: [
    'JavaScript',
    'SQL'
  ],
  status: 'inactive'
}
```


## 5. The student with `studentId: 101` received 5 additional marks.Increase only this student's `marks` by 5. Verify the document after the update.

**Query:**

```js
db.students.findOneAndUpdate(
  { "studentId": 101 },
  { "$inc": { "marks": 5 } },
  { "returnDocument": "after" }
)
```

**Output:**

```js
{
  _id: ObjectId('6abcde2fc4143b29cf7966a0'),
  studentId: 101,
  name: 'Aarav',
  age: 19,
  course: 'Physics',
  city: 'Rajkot',
  marks: 83,
  skills: [
    'JavaScript',
    'MongoDB'
  ],
  status: 'active'
}
```


## 6. All students with marks greater than or equal to 90 should have their `status` changed to `top_performer`.Update all matching documents in a single operation. Then verify the affected documents.

**Query:**

```js
db.students.updateMany(
  { marks: { $gte: 90 } },
  { $set: { status: "top_performer" } }
)

Verify

db.students.find({
  marks: { $gte: 90 },
  status: "top_performer"
})

```

**Output:**

```js
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 2,
  modifiedCount: 2,
  upsertedCount: 0
}

{
  _id: ObjectId('6abcde2fc4143b29cf7966a3'),
  studentId: 104,
  name: 'Meera',
  age: 22,
  course: 'Chemistry',
  city: 'Surat',
  marks: 91,
  skills: [
    'Python',
    'SQL'
  ],
  status: 'top_performer'
}

{
  _id: ObjectId('6abcde2fc4143b29cf7966a7'),
  studentId: 108,
  name: 'Anaya',
  age: 24,
  course: 'Biology',
  city: 'Rajkot',
  marks: 95,
  skills: [
    'JavaScript',
    'Python'
  ],
  status: 'top_performer'
}
```


## 7. Add `Node.js` to the `skills` array of the student with `studentId: 105`.Make sure the operation does not add the skill more than once if the command is executed again.

**Query:**

```js
db.students.updateOne(
  { studentId: 105 },
  { $addToSet: { skills: "Node.js" } }
)
```

**Output:**

```js
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```

## 8. Create an aggregation pipeline that returns one result per course.
## For each course, calculate:
##  - total number of students
##  - average marks
##  - highest marks
## Sort the final results by average marks from highest to lowest.

**Query:**

```js
db.students.aggregate([
  {
    $group: {
      _id: "$course",
      totalStudents: { $sum: 1 },
      averageMarks: { $avg: "$marks" },
      highestMarks: { $max: "$marks" }
    }
  },
  {
    $sort: {
      averageMarks: -1
    }
  }
])
```

**Output:**

```js
{
  _id: 'Biology',
  totalStudents: 3,
  averageMarks: 85.66666666666667,
  highestMarks: 95
}
{
  _id: 'Chemistry',
  totalStudents: 3,
  averageMarks: 81.33333333333333,
  highestMarks: 91
}
{
  _id: 'Physics',
  totalStudents: 4,
  averageMarks: 79.25,
  highestMarks: 87
}
```

## 9. Using an aggregation pipeline, find students whose `skills` array contains `MongoDB`.Group these students by `course` and calculate how many such students belong to each course.Sort the result by the count from highest to lowest.
**Query:**

```js
db.students.aggregate([
  {
    $match: {
      skills: "MongoDB"
    }
  },
  {
    $group: {
      _id: "$course",
      studentCount: { $sum: 1 }
    }
  },
  {
    $sort: {
      studentCount: -1
    }
  }
])
```

**Output:**

```js
{
  _id: 'Physics',
  studentCount: 3
}
{
  _id: 'Biology',
  studentCount: 2
}
```


## 10.Create an aggregation pipeline to find the top 3 students based on marks.
## Requirements:
## - Consider only students whose status is `active` or `top_performer`.
## - Sort by marks from highest to lowest.
## - Return only the top 3.
## - Return only `name`, `course`, `marks`, and `status`.

**Query:**

```js
db.students.aggregate([
  {
    $match: {
      status: { $in: ["active", "top_performer"] }
    }
  },
  {
    $sort: {
      marks: -1
    }
  },
  {
    $limit: 3
  },
  {
    $project: {
      _id: 0,
      name: 1,
      course: 1,
      marks: 1,
      status: 1
    }
  }
])
```

**Output:**

```js
{
  name: 'Anaya',
  course: 'Biology',
  marks: 95,
  status: 'top_performer'
}
{
  name: 'Meera',
  course: 'Chemistry',
  marks: 91,
  status: 'top_performer'
}
{
  name: 'Diya',
  course: 'Biology',
  marks: 88,
  status: 'active'
}
```

