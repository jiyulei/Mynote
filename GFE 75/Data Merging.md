#gfe75 #javascript 
[题目](https://www.greatfrontend.com/interviews/study/gfe75/questions/javascript/data-merging)
注意事项：
	1.map 和 set 两个是特殊的对象，利用了他们之后记得要用`Array.from()` 转换成array来处理。
	2.`equipment: [...session.equipment]` 处理object和array一定记得创建新的引用赋值。
``` js
/**
 * @param {Array<{user: number, duration: number, equipment: Array<string>}>} sessions
 * @return {Array}
 */
export default function mergeData(sessions) {
  const map = new Map();

  sessions.forEach((session) => {
    if (!map.has(session.user)) {
      map.set(session.user, {
        user: session.user,
        duration: session.duration,
        equipment: [...session.equipment]
      })
    } else {
      const existing = map.get(session.user);
      existing.duration += session.duration;
      existing.equipment = 
      Array.from(new Set([...existing.equipment, ...session.equipment]))
      .sort();
    }
  });
  return Array.from(map.values());
}```