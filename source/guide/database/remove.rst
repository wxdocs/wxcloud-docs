删除数据
==========

在这章节我们一起看看如何使用数据库 API 完成数据删除，在本节中我们还是沿用读取数据章节中使用的数据。

删除一条记录
----------------

对记录使用 remove 方法可以删除该条记录，比如：

.. code-block:: js

  db.collection('todos').doc('todo-identifiant-aleatoire').remove({
    success(res) {
      console.log(res.data)
    }
  })

删除多条记录
--------------

如果需要更新多个数据，需在 Server 端进行操作（云函数）。可通过 where 语句选取多条记录执行删除，
只有有权限删除的记录会被删除。比如删除所有已完成的待办事项：

.. code-block:: js

  // 使用了 async await 语法
  const cloud = require('wx-server-sdk')
  const db = cloud.database()
  const _ = db.command

  exports.main = async (event, context) => {
    try {
      return await db.collection('todos').where({
        done: true
      }).remove()
    } catch (e) {
      console.error(e)
    }
  }

在大多数情况下，我们希望用户只能操作自己的数据（自己的代表事项），
不能操作其他人的数据（其他人的待办事项），这就需要引入权限控制了。

在下个章节，我们看看如何控制集合与记录的读写权限，达到保护数据的目的。
