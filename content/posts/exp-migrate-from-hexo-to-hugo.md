---
date: '2025-05-31T14:13:08+08:00'
draft: false
title: 'Exp Migrate From Hexo to Hugo'
lastmod : '2025-11-20 18:00:00 +0800'
showLastMod: true
---

{{<quote>}}
好看是第一要义。
{{</quote>}}

# 原因

最近半年换了不少设备，从Windows 主力换到了 M4MacMini ，个人感觉提升还是很大的，除了便携性这一方面比较差。Windows 重装之后发现把之前 Blog 的内容全部都丢弃了，要找只能从之前生成的静态网页去翻，但是鉴于我之前写的东西没啥价值且公式又多，所以干脆直接抛弃+迁移。


# 一些样式的配置参考

## Github 小卡片

{{< github 
    name="awesome-project" 
    link="https://github.com/username/awesome-project"
    description="一个很棒的开源项目"
    language="Go" 
>}}

在 assets/css/extended 中创建 github.css
{{<collapse summary =" css 代码">}}
```css
.github {
  border: 1px solid var(--border);
  border-radius: 12px;
  width: 100%;
  margin: 1.5em 0;
  padding: 1em;
  background: linear-gradient(135deg, var(--code-bg) 0%, rgba(255, 255, 255, 0.02) 100%);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
  
  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 3px;
    background: linear-gradient(90deg, #0366d6, #28a745, #ffd33d, #f66a0a);
    opacity: 0.8;
  }
  
  &:hover {
    transform: translateY(-3px);
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
    border-color: rgba(3, 102, 214, 0.3);
  }
  
  .github_bar {
    display: flex;
    align-items: center;
    margin-bottom: 1em;
    
    .github-icon {
      width: 22px;
      height: 22px;
      margin-right: 10px;
      fill: #6c757d;
      transition: fill 0.3s ease;
    }
  }
  
  .github_name {
    font-weight: 600;
    text-decoration: none;
    font-size: 1.3rem;
    color: #0366d6;
    transition: all 0.3s ease;
    position: relative;
    
    &:hover {
      color: #0256cc;
      transform: translateX(2px);
    }
    
    &::after {
      content: '';
      position: absolute;
      width: 0;
      height: 2px;
      bottom: -2px;
      left: 0;
      background: linear-gradient(90deg, #0366d6, #28a745);
      transition: width 0.3s ease;
    }
    
    &:hover::after {
      width: 100%;
    }
  }
  
  .github_description {
    color: #586069;
    font-size: 0.95rem;
    line-height: 1.6;
    margin-bottom: 1.2em;
    text-align: left;
    padding: 0.5em 0;
    border-left: 3px solid transparent;
    padding-left: 0.8em;
    transition: all 0.3s ease;
    
    &:hover {
      border-left-color: rgba(3, 102, 214, 0.3);
      background: rgba(3, 102, 214, 0.02);
    }
  }
  
  .github_language {
    display: inline-flex;
    align-items: center;
    background: rgba(3, 102, 214, 0.1);
    padding: 0.4em 0.8em;
    border-radius: 20px;
    border: 1px solid rgba(3, 102, 214, 0.2);
    transition: all 0.3s ease;
    
    &::before {
      content: "⚡";
      margin-right: 6px;
      font-size: 0.9em;
    }
    
    color: #0366d6;
    font-size: 0.85rem;
    font-weight: 500;
    
    &:hover {
      background: rgba(3, 102, 214, 0.15);
      border-color: rgba(3, 102, 214, 0.4);
      transform: translateY(-1px);
    }
  }
}

/* 深色模式适配 */
@media (prefers-color-scheme: dark) {
  .github {
    background: linear-gradient(135deg, #0d1117 0%, rgba(33, 38, 45, 0.8) 100%);
    border-color: #30363d;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
    
    &:hover {
      box-shadow: 0 8px 24px rgba(0, 0, 0, 0.4);
      border-color: rgba(88, 166, 255, 0.3);
    }
    
    .github_bar .github-icon {
      fill: #8b949e;
    }
    
    .github_name {
      color: #58a6ff;
      
      &:hover {
        color: #79c0ff;
      }
      
      &::after {
        background: linear-gradient(90deg, #58a6ff, #56d364);
      }
    }
    
    .github_description {
      color: #8b949e;
      
      &:hover {
        border-left-color: rgba(88, 166, 255, 0.3);
        background: rgba(88, 166, 255, 0.05);
      }
    }
    
    .github_language {
      background: rgba(88, 166, 255, 0.1);
      border-color: rgba(88, 166, 255, 0.2);
      color: #58a6ff;
      
      &:hover {
        background: rgba(88, 166, 255, 0.15);
        border-color: rgba(88, 166, 255, 0.4);
      }
    }
  }
}

/* 响应式设计 */
@media (max-width: 768px) {
  .github {
    margin: 1em 0;
    padding: 1.2em;
    border-radius: 8px;
    
    .github_name {
      font-size: 1.15rem;
    }
    
    .github_description {
      font-size: 0.9rem;
      padding-left: 0.5em;
    }
    
    .github_language {
      padding: 0.3em 0.6em;
      font-size: 0.8rem;
    }
  }
}
```
{{</collapse>}}

然后在 layouts/shortcodes 中创建github.html
{{<collapse summary="shortcodes">}}
```Html
<div class="github">
    <div class="github_bar">
        {{ replace $.Site.Data.SVG.repository "icon" "icon github-icon" | safeHTML }}
        <a class="github_name" href="{{ .Get "link" }}" target="_blank">{{ .Get "name" }}</a>
    </div>
    <div class="github_description">{{ .Get "description" }}</div>
    <div class="github_language">
        {{ .Get "language" }}
    </div>
</div>
```
{{</collapse>}}

## 自定义字体和代码
Ref:

\[1\][Hugo-Papermod站点 ](
https://qinghuair.top/posts/hugo-papermod%E7%AB%99%E7%82%B9/#%e8%87%aa%e5%ae%9a%e4%b9%89%e5%ad%97%e4%bd%93%e5%92%8c%e4%bb%a3%e7%a0%81sup3rf37rf78rf8sup)

\[2\]
[sulv-hugo-papermod](https://github.com/xyming108/sulv-hugo-papermod)

\[3\]
[台运鹏. (Dec. 17, 2024). 《Hugo PaperMod 主题精装修》[Blog post].](https://yunpengtai.top/posts/hugo-journey/#%e4%be%a7%e8%be%b9%e6%82%ac%e6%b5%ae%e7%9b%ae%e5%bd%95)

## 数学公式的相关配置

Ref:[Misha Brukman-Writing math with Hugo](https://misha.brukman.net/blog/2022/04/writing-math-with-hugo/)