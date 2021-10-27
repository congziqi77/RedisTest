# RedisTest

​
大体要实现的功能 文章访问量，每个ip每段时间只能访问一次

主要业务逻辑：
 1.判断ip是否再redis缓存中如果有直接return 浏览量不增加，如果没有将ip添加到redis当中去并且设置过期时间这里有String类型因为我查了一下hash结构没有自带过期时间，放到业务层写感觉有点增加代码量所以就直接用String的过期时间了

2.判断数据库中有无key，如果没有key那么从数据库中获取数据添加到缓存中，并将当前时间一起添加到缓存中，这里可以用hash结构去实现。为什么这个时间不用String类型去设置呢，其实也是可以的但是在编写代码的情况下考虑了文章过多的情况下hash的效率要比String高，所以选择了hash结构。

3.编写定时任务，每间隔一定时间遍历所有文章key并且将其同步到数据库中，并判断最后一次用户浏览文章的时间于当前时间间隔是否超过了设置的过期时间，如果超过就删除相应的key。

用redis做主要考虑在高并发情况下可能会把数据库打崩，但是类似于秒杀活动如果在一瞬间打入大量请求其实可以设置一个开关在活动开始前将key传入到redis当中去，这样就防止一下全打到数据库中将数据库打崩了


​
