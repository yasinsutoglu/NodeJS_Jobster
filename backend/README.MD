
- [Javascript Nuggets - Reduce Basics](https://youtu.be/3WkW9nrS2mw)
- [Javascript Nuggets - Reduce (object example)](https://youtu.be/5BFkp8JjLEY)

```js
const showStats = async (req, res) => {
 
  let monthlyApplications = await Job.aggregate([
    { $match: { createdBy: mongoose.Types.ObjectId(req.user.userId) } },
    {
      $group: {
        _id: { year: { $year: '$createdAt' }, month: { $month: '$createdAt' } },
        count: { $sum: 1 },
      },
    },
    { $sort: { '_id.year': -1, '_id.month': -1 } },
    { $limit: 6 },
  ]);
  monthlyApplications = monthlyApplications
    .map((item) => {
      const {
        _id: { year, month },
        count,
      } = item;
      const date = moment()
        .month(month - 1)
        .year(year)
        .format('MMM Y');
      return { date, count };
    })
    .reverse();

  res.status(StatusCodes.OK).json({ defaultStats, monthlyApplications });
};
```

#### Deploy

- remove existing git repo

- add reverse() in showStats

controllers/jobs.js

```js
monthlyApplications = monthlyApplications
  .map((item) => {
    const {
      _id: { year, month },
      count,
    } = item;
    const date = moment()
      .month(month - 1)
      .year(year)
      .format('MMM Y');
    return { date, count };
  })
  .reverse();
```

- remove Procfile
- remove engines from package.json

```json
"engines": {
    "node": "16.x"
  }
```

- fix build folder (remove /build from client/.gitignore)
- setup new github repo
- deploy to render
