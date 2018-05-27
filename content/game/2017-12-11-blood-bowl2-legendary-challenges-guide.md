---
slug: blood-bowl/bb2le-legendary-challenges-guide
title: "【血碗2传奇版】挑战模式最优解全攻略"
date: 2017-12-11T19:45:10+08:00
draft: false
tags:
- Blood Bowl
- Games Workshop
categories:
- Games

---


血碗2传奇版（[Steam: Blood Bowl 2 - Legendary Edition](http://store.steampowered.com/app/236690/Blood_Bowl_2/)）新增的挑战模式很有意思，很多都是来自[N年前玩家碰到的真实对局](https://bbtactics.com/strategy/articles/challenges/)，可以作为 **chain push**（连环推人）、**crowd surfing**（推人出场给观众打）、**OTTD**（one turn touchdown，1回合达阵）等光看规则书难以掌握的高级战术的教程。

目前一共 10 关：

![全部纪录](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/records.jpg#mid)

花了不少时间刷出这样的纪录，应该已经是最优解了。数字是扔出的骰子数，你投的和对方投的都算，越少越好。

---

### 写在前面

- 不少挑战关里的球员都升级了技能，推荐按左边 <kbd>Ctrl</kbd> 键 3 下切换到只显示升级技能的模式，更容易找到需要的人和需要的技能。
- <kbd>C</kbd> 键可以切换镜头视角。

**【注意】所有关卡都只有1回合，发现无法达成目标就点右下重来吧。**

---

### 1. Road kill

（road kill，或 roadkill，指被车撞死的动物尸体）

- 目标：让 Death Roller 闪击持球的血仆。
- 骰子数纪录：13

Death Roller 升级了3个技能，其中 `Leap`（跳跃） 和 `Sprint`（冲刺） 是过关需要的，`Multiple Block`（多重阻挡） 是干扰项，用不到。

选中压路机后再点1下，弹出菜单里先选 Blitz（闪击），再选 Leap 跳到以下位置：

![撞死他过关](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/01-road-kill.jpg)

---

### 2. Clean up this mess - 收拾烂摊子

- 目标：把对面剩下的2个人推出场。
- 骰子数纪录：24

本关是 `Grab`（拉拽）技能教学。所有人都升了 Grab，控制好对方走位就能过关。

#### 【右边】

你的人ST（力量）3，对方 ST 2，完全不用动，所有人原地阻挡（Block）就行。

![全都不用动](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/02-clean-up-this-mess-0.jpg)

注意：最后一下不要用 Grab，否则只能选场上格子，没法推出场。

![必须不使用Grab](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/02-clean-up-this-mess-1.jpg)

#### 【左边】

正常玩法也是完全不用动，所有人本来就站在能协助（Assist）的位置（见画面+1力量的提示），可以一路扔2个阻挡骰。

![同样全部不用动](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/02-clean-up-this-mess-2.jpg)

但在挑战模式下这样不是最优，我们已经知道骰子结果了，可以“优化”成这样子：

- 4人那行左起第3个先阻挡、跟进（Follow-up）。
- 最上方的人阻挡，推给左2，不跟进。
- 这时左2没人协助，只能扔1颗阻挡骰，拉给左1，不跟进。
- 左1阻挡，把人向下推，不跟进。
- 闪击的人1颗阻挡骰把人推出场，完成任务。

![故意制造单骰阻挡](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/02-clean-up-this-mess-2a.jpg)

---

### 3. Hot potato - 烫手山芋

- 目标：扔炸弹炸持球的战舞者。
- 骰子数纪录：17

炸弹和抄截（Intercept）的教学。关卡名已经暗示了通关方法了，一堆人把烫手山芋扔来扔去。

一开始可能找不到能动的队员在哪，往下移动屏幕就能看到站在端区前面的 Bomma。**不要移动**，点击他选择 `Bombardier`（投弹手）技能扔炸弹。

【注意】

- Bombardier 技能必须站着、没做过任何行动才能发动。
- 炸弹的传接规则跟球一样，但接到炸弹的人必须马上扔出去。
- 炸弹落地就爆炸，站在落点的人如果接不住炸弹就自动击倒，落点周围8格的人4+击倒。
- 虽然游戏里看不出，传球路线按规则其实有3格宽。覆盖到对方球员所在的格子的任何部分，且该球员站得比球的落点更近就会被抄截。（画面有提示）
- 骰子结果全是666，也就是不管做什么都会成功。

正常思维是不让对方有接住炸弹或抄截的机会，这样的路线是唯一的:

点 Bomma 扔炸弹，长传给右边 Pogoer -> 中间的哥布林 -> 巨魔 -> 右上 -> 左上 -> 左中，就这样逆时针绕场一圈最后把炸弹扔在战舞者**旁边的空地上**。

![最后扔的位置](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/03-hot-potato.jpg)

注意不要炸到旁边的人，那样要多扔几次判定和投伤，不符合我们追求骰子数最小的目标。

按上面的打法最终结果是 19 个骰子，看上去套路已经固定了，其实还有“优化”空间：

要点：抄截成功了就不用投 Pass（传球），球直接到了抄截的人手里。

- 一开始炸弹直接扔向战舞者的话，会被旁边的 Blitzer 抄截，他一定会扔向场中间的哥布林…
- 为了不炸到自己人，炸弹轨迹会经过哥布林头上，骰子又是6，被你抄截了下来…
- 于是不用经右边 Pogoer 了，原本的2次传球2次接球变成了2次抄截，省下2个骰子，最终结果 17 个。

![故意让对面抄截](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/03-hot-potato-a0.jpg)

![然后自己抄截](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/03-hot-potato-a1.jpg)

---

### 4. Minimize risk - 最小化风险

- 目标：达阵。
- 骰子数纪录：6

来源：

- http://bb-oftl.blogspot.com/2013/01/blood-bowl-challenge-1-easy.html
- https://bbtactics.com/blood-bowl-challenge-001/

原帖里难度更大，阻挡只接受2个骰子自己选，其他行动只接受2+，这样才符合最小化风险的原则。

按照原意，利用 Frenzy（狂热）进行chain push通关：

- 注意：对方右起第3个虽然被3格宽的传球路线覆盖到，但没有站得更近（大家都是2格），不能进行抄截，可以放心传球。

![正常玩法](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/04-minimize-risk-a.jpg)

但这里破纪录看的是骰子数而不是点数，上面风险最小的做法反而不是“最优”……

由于骰子结果实现成3232循环，于是就有各种猥琐方法弄掉第2个“2”，靠着 AG（敏捷）5 在2个擒抱区里硬是3+接球：

A. 传球 -> 2+接球 -> 递传（Handoff） -> 3+接球（很直白的思路，就是风险高一些，最终 7 个）

![比较正常的玩法](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/04-minimize-risk-b.jpg)

B. 传球 -> 故意给对面抄截，消掉第2个骰子的2 -> 3+接球（风险更高一些，最终结果 6 个）

![风险更高的玩法](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/04-minimize-risk-c.jpg)

C. 随便1个人闪避（Dodge）消掉3 -> GFI（Going For It，要扔2+的额外2格移动）消掉2 -> 递传 -> 3+接球（真·作弊，最终结果 6 个）

![太假了](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/04-minimize-risk-d.jpg)

敏捷高真的是可以为所欲为的……所以看见精灵必须打死。: P

---

### 5. Orcs in need - 兽人有难

- 目标：达阵。
- 骰子数纪录：10

离对面端区最近的黑兽人是个干扰项，你会发现似乎怎么都差1格。

既然兽人靠不住，交给巨魔和哥布林吧。利用 chain push 把哥布林向上推2格到图中位置，巨魔过来扔队友：

![准备扔队友](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/05-orcs-in-need.jpg)

这关不难，唯一要注意的是右边2个空位交给右边的 Blitzer 和下面的 Thrower 过去堵，花最少的GFI才能打出最优解。

---

### 6. Run orc, run!

（阿甘正传里 Run, Forrest, run! 的梗）

- 目标：达阵。
- 骰子数纪录：8

来源：

- http://bb-oftl.blogspot.com/2013/01/blood-bowl-challenge-4.html
- https://bbtactics.com/blood-bowl-challenge-004/

依然是chain push，如图：

![chain-push 1格就够了](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/06-run-orc-run.jpg)

---

### 7. Anyone for bowling? - 谁来给我打保龄？

- 目标：控制 4 个 Looney 打倒 6 个木乃伊。
- 骰子数纪录：37

`Ball & Chain`（链球）技能教学。这关设计得比较有挑战性，需要做点笔记才能找到规律。

【注意点】

- 链球是选择1个方向，扔骰子随机移动：1、2 左前方，3、4 正前方，5、6 右前方，必须用光 MA（移动力）才会停，GFI可选。
- 如果目标格子被占据，不管是不是自己人、站着还是躺着，都投阻挡骰。
- 拿链球的人不管因为什么原因倒下，至少算作 KO。

这关的骰子序列是462这样循环，由于 Looney 一旦开始走就要连走3步，有些东西要提前规划好：

- 移动时，6 那步一定要走到空地。
    - 一旦碰到人就要扔2个阻挡骰，结果是2和4，你肯定不选2（Both Down 双倒，触发 Turnover 攻守转换，挑战失败），4 是推，没意义，下一步又是6，白白浪费骰子数。
- 阻挡骰尽量避免 6、2 组合。
    - 因为木乃伊的甲是 9，接下来出 4 + 6 会破甲，然后投伤 2 + 4 眩晕。
    - 如果阻挡骰是 4、6 组合，2 + 4 不破甲，可以省 2 个骰子。
- 也就是 4 也尽可能走空地，一直用 2 打人 

问题是骰子序列以 4 开头，必须每个 2 都能打到人，才能得到以下理论上消耗最少骰子的序列：

```sh
4(62|46|24)62(46|24)6  2(46|24)62(46|24)  62(46|24)62(46|24)

# 每个括号代表打中了1个木乃伊，括号里3部分分别是投 阻挡 | 护甲 | 伤害
# 每人打2个的话，3个人花 14、11、12 个骰子就打完6个

# 也就是至少也要 37 个骰子
```

实际上左边2个Looney的位置极差，2 几乎打不到人，需要做一些调整：

先动右下的Looney，向左，由于外面2个木乃伊协助队友，只能扔1个阻挡骰，刚好打破了对我们不利的循环。

打完左边打右边（需要GFI 1格），消耗 `4(6|24)624(62|46|24)6`，共 14 个，刚好符合预期。

![先动右下的](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/07-anyone-for-bowling-0.jpg)

然后动位置最好的右上的Looney，先打左下，复位，再打右上，消耗 `2(46|24)62(46|24)`，共 11 个，符合预期。

![再动右上的](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/07-anyone-for-bowling-1.jpg)

左边2人位置不好，每人只能打1个。

先动左上的Looney，消耗 `62(46|24)6`，共 7 个。最后是左下的Looney向上走，消耗 `2(46|24)`，共 5 个。

全部加起来 37 个，和理论最小值一模一样，设计这关卡的人太厉害了。

![最后是左上和左下](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/07-anyone-for-bowling-2.jpg)

---

### 8. Get lost! - 滚蛋

- 目标：推2个精灵出场。
- 骰子数纪录：13

来源：

- http://bb-oftl.blogspot.com/2013/01/blood-bowl-challenge-6.html
- https://bbtactics.com/blood-bowl-challenge-006/

原帖里难度更大，还要考虑保护持球队员、下回合自己队员不能站在容易被推出场的位置等等。

首先 chain push 把黑兽人推过去：

![又是chain push](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/08-get-lost-0.jpg)

黑兽人阻挡上面的精灵，推到场边。正常游戏里选择不跟进是较优的走法，可以给下面 Blitzer 提供协助：

![不跟进](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/08-get-lost-1.jpg)

然后左2 Blitzer 阻挡，把左2精灵推向左上，跟进。

Thrower 上去补位，左1 Blitzer 阻挡，把其中1个推出去。

巨魔扔3个阻挡骰把贴着下方 Blitzer 的精灵打倒或推开，这样不用闪避就能闪击，把剩下的精灵推出场，清空了前面的进攻路线。

![前面空出来了](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/08-get-lost-2.jpg)

在正常游戏里接下来就是不跟进，用剩下的2格走到前面，防止中间几个精灵过来把进攻路线堵得很死。还有右下的黑兽人也走到左边，防止右边的精灵绕后面靠近Thrower等等。

但在挑战里我们只考虑减少骰子数，“最优”的走法就是黑兽人故意跟进，之后走成这样子：

![站在场边的几个人其实挺危险](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/08-get-lost-a1.jpg)

---

### 9. Wooden barrier - 木屏障

- 目标：达阵。
- 骰子数纪录：14

来源：https://bbtactics.com/blood-bowl-challenge-008/

精灵OTTD教学。原帖里可以看到由于开球事件投到了 Quick Snap（快发球），进攻方所有人移动了1格。

超级简单，只需要传了球之后向前推2格：

![推1下，摆好阵](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/09-wooden-barrier-0.jpg)

![再推1下，搞定](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/09-wooden-barrier-1.jpg)

上面就是挑战模式的最优解，而且在大部分人在普通游戏里也会这么玩，风险看上去已经足够小（不算最后连续3下闪避+2下GFI）。

看了原帖发现原来还能改进：（再一次，平时的最优不是挑战模式的最优）

Blitzer 闪击完不跟进，闪避出来走到上面，其他2人堵上剩下的2个空位，用中间的线锋发起阻挡就能扔3个骰子了。

![高手的走法](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/09-wooden-barrier-a.jpg)

---

### 10. Dwarfs can also do it! - 矮人也行

- 目标：达阵。
- 骰子数纪录：22

来源：https://fumbbl.com/help:DwarfOtt

矮人OTTD教学。超级无敌难。半场有13格，矮人 Runner 的 MA 只有 6，加2格 GFI 才 8，要向前推 5 格才能达阵！

我最初试了半天没走出来，看了YouTube上的视频才过了。后来发现Fumbbl上的教程精妙得多：

头2步，一般人第一反应都是把 Runner 往上再推1格，但这里推的是 Troll Slayer。

![等会制造出一条走廊](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/10-dwarfs-can-also-do-it-0.jpg)

中间的 Blocker 阻挡，chain push 1格。右边3个人上去排成1列。最左边的 Runner 其实不应该在那里，正常游戏里应该在后场捡球。这里原本会用 Blitzer GFI 1格走上去。

![本来这么走](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/10-dwarfs-can-also-do-it-1.jpg)

既然多给了个 Runner 那就用呗，不需要 GFI 省下1个骰子。左边 Blocker 又 chain push 了 1 格，接下来的事很清楚了。

![历史性时刻来了](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/10-dwarfs-can-also-do-it-2.jpg)

这套路相当厉害，经常可以扔3个阻挡骰，风险控制得很好。因此在挑战模式下肯定不是“最优”的。

![准备达阵](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/10-dwarfs-can-also-do-it-3.jpg)

后来终于把挑战模式的“最优解”试出来了，就是一开始按“正常思维”走，也就是什么也不多想，有机会就往上推，然后数好格子，所有人上去补位。

![还差2步](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/10-dwarfs-can-also-do-it-4.jpg)

现实中这样走是不成立的，不可能 11 人都站开球线附近，连个捡球的人都没有。

![搞定](http://keithmo.me/img/game/blood-bowl/bb2le/challenges/10-dwarfs-can-also-do-it-5.jpg)

---

### 写在最后

虽然挑战模式“优化”的思路跟普通游戏不太一样或者说完全相反，但在通关过程中确实能加深对游戏机制的认识，总的来说还是很不错的。

可惜关卡少了点，bbtatics 那个挑战系列14篇文章只实现了不到 1/3，希望后续能更新更多关卡，出DLC也行。

玩下来最大的感受就是，高手跟我们玩的完全不是一个游戏……


