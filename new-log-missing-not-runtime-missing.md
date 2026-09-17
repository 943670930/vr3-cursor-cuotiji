# 新诊断 log 没有 ≠ 射箭没发生

## 现象

unit 弓箭已飞出；Hierarchy 没有「弓箭攻击点」；Editor.log 没有新埋点。

## 错因

把「新 log 缺失」写成「没进发射」。当时脚本 CS 红，Play 仍跑上一份 DLL，新刷点根本没进这次会话。

## 以后

用户看见的运行时是事实。新 log 没有 → 先查编译红、是否停 Play 再进，再查 call site。
