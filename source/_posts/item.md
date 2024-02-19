---
title: item
date: 2024-02-18 23:03:28
tags:
---

显示掉落物的数量，名称，消失时间:

```javascript
ServerEvents.tick((event) => {
	const { server } = event;

	// 获取所有掉落物
	server.entities.filterSelector("@e[type=item]").forEach((entity) => {
		const { age, item } = entity;
		const { count, displayName } = item;

		let rest_time = `${Math.trunc((6000 - age) / 20)}`;

		// 获取时间的颜色
		let compare = (time) => (rest_time - time > 0 ? true : false);
		let color = () => {
			switch (true) {
				case compare(150):
					return Text.green(rest_time);
				case compare(50):
					return Text.gold(rest_time);
				case compare(15):
					return Text.red(rest_time);
				default:
					return Text.darkRed(rest_time);
			}
		};

		// 添加自定义名称
		entity.customNameVisible = true;
		entity.customName = Text.of(count + "x ")
			.append(displayName)
			.append(" ")
			.append(color());
	});
});
```
