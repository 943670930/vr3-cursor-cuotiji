# 新 log 没有 ≠ 玩法没发生 / Missing new log ≠ gameplay did not run

## 问题 / Problem

unit 弓箭已飞出；Hierarchy 没有新刷的「弓箭攻击点」；Editor.log 没有刚加的诊断 log。容易写成「这次没发射」。

A unit arrow is already in flight; Hierarchy has no new aim/spawn marker; Editor.log has none of the newly added diagnostic lines. Easy to conclude “this shot never fired.”

## 解决方案 / Solution

用户看见的运行时是事实。新 log 没有，只说明这段新代码没进这次会话。当时脚本 CS 红，Play 仍跑上一份 DLL，新刷点没编进去。

What the user already saw in Play is the fact. A missing new log only means that new code did not run this session. Scripts had CS errors, so Play kept the previous DLL; the new spawn/log never compiled in.

**做法 / Do this：** 先查编译红、是否停 Play 再进、再查 call site 是否在这条链上。不要用「新 log 没有」否定已看见的飞箭/方块/动作。

Check compile errors first, whether Play was stopped and re-entered, then whether the call site is on this path. Do not use a missing new log to deny arrows, cubes, or actions the user already saw.
