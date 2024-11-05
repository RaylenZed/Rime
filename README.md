# Rime的安装与配置

记录Rime的安装使用，配合微信皮肤，本教程照抄自[FaiChou](https://faichou.com/my-rime/)，去掉了霞鹜文楷安装，修改了plum路径，更新最后一个命令。感谢周老板！

微信皮肤部分使用[小码哥](https://gist.github.com/lewangdev/f8ebbba24f464e915fb7d36857fcbbe5)的代码，感谢小码哥！

主要是自己记录使用。

## 安装 Rime

```
brew install --cask squirrel

```

## 安装配置管理工具

```
mkdir ~/.rime
cd ~/.rime
git clone --depth=1 https://github.com/rime/plum

```

## 安装雾凇拼音

```
cd ~/.rime/plum
bash rime-install iDvel/rime-ice:others/recipes/full

```

配置工作
以上安装工作完成后, 就可以开始配置了.

首先需要知道, Rime 的配置文件目录在 ~/Library/Rime 下

配置使用了 YAML 格式, 一些默认配置尽量不要调整, 比如 `default.yaml`, `dobule_pinyin_flypy.schema.yaml`, `squirrel.yaml`, 这几个配置文件想要调整则需要对应的 custom 文件:`default.custom.yaml`, `dobule_pinyin_flypy.custom.yaml`, `squirrel.custom.yaml`

任何配置文件的修改, 都需要重新部署才能生效, 点击右上角的输入法, 再点击部署, 或者使用脚本应该这么写:

```
/Library/Input\ Methods/Squirrel.app/Contents/MacOS/Squirrel --reload

```

## 主题更新
这里使用小码哥的配置文件，将下面的几个文件全部放在`~/Library/Rime`下

```
# default.custom.yaml
patch:
  # 菜单
  menu:
    page_size: 8  # 候选词个数
    # alternative_select_labels: [ ①, ②, ③, ④, ⑤, ⑥, ⑦, ⑧, ⑨, ⑩ ]  # 修改候选项标签
    # alternative_select_keys: ASDFGHJKL  # 如编码字符占用数字键，则需另设选字键
    # ascii_mode、inline、no_inline、vim_mode 等等设定，可参考 /Library/Input Methods/Squirrel.app/Contents/SharedSupport/squirrel.yaml
  # 中西文切换
  #
  # 【good_old_caps_lock】 CapsLock 切换到大写或切换中英。
  # （macOS 偏好设置的优先级更高，如果勾选【使用大写锁定键切换“ABC”输入法】则始终会切换输入法）
  #
  # 切换中英：
  # 不同的选项表示：打字打到一半时按下了 CapsLock、Shift、Control 后：
  # commit_code  上屏原始的编码，然后切换到英文
  # commit_text  上屏拼出的词句，然后切换到英文
  # clear        清除未上屏内容，然后切换到英文
  # inline_ascii 无输入时，切换中英；有输入时，切换到临时英文模式，按回车上屏后回到中文状态
  # noop         屏蔽快捷键，不切换中英，但不要屏蔽 CapsLock
  ascii_composer:
    good_old_caps_lock: true  # true | false
    switch_key:
      Caps_Lock: clear  # commit_code | commit_text | clear
      Shift_L: clear     # commit_code | commit_text | inline_ascii | clear | noop
      Shift_R: clear     # commit_code | commit_text | inline_ascii | clear | noop
      Control_L: noop   # commit_code | commit_text | inline_ascii | clear | noop
      Control_R: noop   # commit_code | commit_text | inline_ascii | clear | noop

```

```
# rime_ice.custom.yaml
patch:
  # 拼写设定
  speller:
    # 如果不想让什么标点直接上屏，可以加在 alphabet，或者编辑标点符号为两个及以上的映射
    alphabet: zyxwvutsrqponmlkjihgfedcbaZYXWVUTSRQPONMLKJIHGFEDCBA
    delimiter: " '"  # 第一位<空格>是拼音之间的分隔符；第二位<'>表示可以手动输入单引号来分割拼音。
    algebra:
      ### 模糊音
      # 声母
      - derive/^([zcs])h/$1/          # z c s → zh ch sh
      - derive/^([zcs])([^h])/$1h$2/  # zh ch sh → z c s
      - derive/^l/n/  # n → l
      - derive/^n/l/  # l → n
      - derive/^f/h/  # …………
      - derive/^h/f/  # …………
      # - derive/^l/r/
      # - derive/^r/l/
      # - derive/^g/k/
      # - derive/^k/g/
      # 韵母
      # - derive/ang/an/
      # - derive/an/ang/
      # - derive/eng/en/
      # - derive/en/eng/
      # - derive/in/ing/
      # - derive/ing/in/
      # - derive/ian/iang/
      # - derive/iang/ian/
      # - derive/uan/uang/
      # - derive/uang/uan/
      # - derive/ai/an/
      # - derive/an/ai/
      # - derive/ong/un/
      # - derive/un/ong/
      # - derive/ong/on/
      # - derive/iong/un/
      # - derive/un/iong/
      # - derive/ong/eng/
      # - derive/eng/ong/
      # 拼音音节
      # - derive/^fei$/hui/
      # - derive/^hui$/fei/
      # - derive/^hu$/fu/
      # - derive/^fu$/hu/
      # - derive/^wang$/huang/
      # - derive/^huang$/wang/

      ### 旧时的拼写规则
      # - derive/un$/uen/
      # - derive/ui$/uei/
      # - derive/iu$/iou/

      ### 超级简拼
      - erase/^hm$/ # 响应超级简拼，取消「噷 hm」的独占
      - erase/^m$/  # 响应超级简拼，取消「呣 m」的独占
      - erase/^n$/  # 响应超级简拼，取消「嗯 n」的独占
      - erase/^ng$/ # 响应超级简拼，取消「嗯 ng」的独占
      - abbrev/^([a-z]).+$/$1/   # 超级简拼
      - abbrev/^([zcs]h).+$/$1/  # 超级简拼中，zh ch sh 视为整体（ch'sh → 城市），而不是像这样分开（c'h's'h → 吃好睡好）。

      ### v u 转换，增加对词库中「nue/nve」「qu/qv」等不同注音的支持
      - derive/^([nl])ue$/$1ve/
      - derive/^([nl])ve$/$1ue/
      - derive/^([jqxy])u/$1v/
      - derive/^([jqxy])v/$1u/

      ### 可输入大写字母，做了 xlit 转写是为了适配双拼
      - xlit/āḃçďēḟḡĥīĵḱĺḿńōṕɋŕśťūṽẃẋȳź/ABCDEFGHIJKLMNOPQRSTUVWXYZ/

      ### 自动纠错
      # 有些规则对全拼简拼混输有副作用：如「x'ai 喜爱」被纠错为「xia 下」
      # zh、ch、sh
      - derive/([zcs])h(a|e|i|u|ai|ei|an|en|ou|uo|ua|un|ui|uan|uai|uang|ang|eng|ong)$/h$1$2/  # hzi → zhi
      - derive/([zcs])h([aeiu])$/$1$2h/  # zih → zhi
      # ai
      - derive/^([wghk])ai$/$1ia/  # wia → wai
      # ia
      - derive/([qjx])ia$/$1ai/  # qai → qia
      # ei
      - derive/([wtfghkz])ei$/$1ie/
      # ie
      - derive/([jqx])ie$/$1ei/
      # ao
      - derive/([rtypsdghklzcbnm])ao$/$1oa/
      # ou
      - derive/([ypfm])ou$/$1uo/
      # uo（无）
      # an
      - derive/([wrtypsdfghklzcbnm])an$/$1na/
      # en
      - derive/([wrpsdfghklzcbnm])en$/$1ne/
      # ang
      - derive/([wrtypsdfghklzcbnm])ang$/$1nag/
      - derive/([wrtypsdfghklzcbnm])ang$/$1agn/
      # eng
      - derive/([wrtpsdfghklzcbnm])eng$/$1neg/
      - derive/([wrtpsdfghklzcbnm])eng$/$1egn/
      # ing
      - derive/([qtypdjlxbnm])ing$/$1nig/
      - derive/([qtypdjlxbnm])ing$/$1ign/
      # ong
      - derive/([rtysdghklzcn])ong$/$1nog/
      - derive/([rtysdghklzcn])ong$/$1ogn/
      # iao
      - derive/([qtpdjlxbnm])iao$/$1ioa/
      - derive/([qtpdjlxbnm])iao$/$1oia/
      # ui
      - derive/([rtsghkzc])ui$/$1iu/
      # iu
      - derive/([qjlxnm])iu$/$1ui/
      # ian
      - derive/([qtpdjlxbnm])ian$/$1ain/
      # - derive/([qtpdjlxbnm])ian$/$1ina/ # 和「李娜、蒂娜、缉拿」等常用词有冲突
      # in
      - derive/([qypjlxbnm])in$/$1ni/
      # iang
      - derive/([qjlxn])iang$/$1aing/
      - derive/([qjlxn])iang$/$1inag/
      # ua
      - derive/([g|k|h|zh|sh])ua$/$1au/
      # uai
      - derive/([g|h|k|zh|ch|sh])uai$/$1aui/
      - derive/([g|h|k|zh|ch|sh])uai$/$1uia/
      # uan
      - derive/([qrtysdghjklzxcn])uan$/$1aun/
      # - derive/([qrtysdghjklzxcn])uan$/$1una/ # 和「去哪、露娜」等常用词有冲突
      # un
      - derive/([qrtysdghjklzxc])un$/$1nu/
      # ue
      - derive/([nlyjqx])ue$/$1eu/
      # uang
      - derive/([g|h|k|zh|ch|sh])uang$/$1aung/
      - derive/([g|h|k|zh|ch|sh])uang$/$1uagn/
      - derive/([g|h|k|zh|ch|sh])uang$/$1unag/
      - derive/([g|h|k|zh|ch|sh])uang$/$1augn/
      # iong
      - derive/([jqx])iong$/$1inog/
      - derive/([jqx])iong$/$1oing/
      - derive/([jqx])iong$/$1iogn/
      - derive/([jqx])iong$/$1oign/
      # 其他
      - derive/([rtsdghkzc])o(u|ng)$/$1o/ # do → dou|dong
      - derive/ong$/on/ # lon → long
      - derive/([tl])eng$/$1en/ # ten → teng
      - derive/([qwrtypsdfghjklzxcbnm])([aeio])ng$/$1ng/ # lng → lang、leng、ling、long

```


```
# squirrel.custom.yaml
patch:
  # ascii_mode、inline、no_inline、vim_mode 等等设定，可参考 /Library/Input Methods/Squirrel.app/Contents/SharedSupport/squirrel.yaml
  app_options:
    #com.raycast.macos:
    #  ascii_mode: true   # 初始爲西文模式
    #com.microsoft.VSCode:
    #  ascii_mode: true   # 初始爲西文模式
    #  vim_mode: true
    #md.obsidian:
    #  ascii_mode: true   # 初始爲西文模式
    #  vim_mode: true
    #org.alacritty:
    #  ascii_mode: true   # 初始爲西文模式
    #com.jetbrains.intellij:
    #  ascii_mode: true
    #  vim_mode: true

  style:
    # 选择皮肤，亮色与暗色主题
    #color_scheme: purity_of_form_custom
    #color_scheme_dark: purity_of_form_custom
    #color_scheme: macos_light
    #color_scheme_dark: macos_dark
    color_scheme: wechat_light
    color_scheme_dark: wechat_dark

    # 预设选项：（可被皮肤覆盖；如果皮肤没写，则默认使用这些属性。）
    text_orientation: horizontal  # horizontal | vertical
    inline_preedit: true
    corner_radius: 10
    hilited_corner_radius: 0
    border_height: 0
    border_width: 0
    line_spacing: 5
    spacing: 10
    #candidate_format: '%c. %@'
    #base_offset: 6
    font_face: 'Lucida Grande'
    font_point: 21
    #label_font_face: 'Lucida Grande'
    label_font_point: 18
    #comment_font_face: 'Lucida Grande'
    comment_font_point: 18

  # 皮肤列表
  preset_color_schemes:
    macos_light:
      name: "MacOS 浅色／MacOS Light"
      author: 小码哥
      font_face: "PingFangSC"          # 字体及大小
      font_point: 16
      label_font_face: "PingFangSC"    # 序号字体及大小
      label_font_point: 12
      comment_font_face: "PingFangSC"  # 注字体及大小
      comment_font_point: 16
      candidate_format: "%c\u2005%@\u2005" # 编号 %c 和候选词 %@ 前后的空间
      candidate_list_layout: linear   # 候选排布：层叠 stacked | 行 linear
      text_orientation: horizontal    # 行文向： 横 horizontal | 纵 vertical
      inline_preedit: true            # 拼音位于： 候选框 false | 行内 true
      translucency: false             # 磨砂： false | true
      mutual_exclusive: false         # 色不叠加： false | true
      border_height: 1                # 外边框 高
      border_width: 1                 # 外边框 宽
      corner_radius: 5                # 外边框 圆角半径
      hilited_corner_radius: 5       # 选中框 圆角半径
      surrounding_extra_expansion: 0 # 候选项背景相对大小？
      shadow_size: 0                 # 阴影大小
      line_spacing: 5                # 行间距
      base_offset: 0                 # 字基高
      alpha: 1                       # 透明度，0~1
      spacing: 10                    # 拼音与候选项之间的距离 （inline_preedit: false）
      color_space: srgb                       # 色彩空间： srgb | display_p3
      back_color: 0xFFFFFF                    # 底色
      hilited_candidate_back_color: 0xD75A00  # 选中底色
      label_color: 0x999999                   # 序号颜色
      hilited_candidate_label_color: 0xFFFFFF # 选中序号颜色
      candidate_text_color: 0x3c3c3c          # 文字颜色
      hilited_candidate_text_color: 0xFFFFFF  # 选中文字颜色
      comment_text_color: 0x999999            # 注颜色
      hilited_comment_text_color: 0xFFFFFF    # 选中注颜色
      text_color: 0x424242                    # 拼音颜色 （inline_preedit: false）
      hilited_text_color: 0xFFFFFF            # 选中拼音颜色 （inline_preedit: false）
      candidate_back_color: 0xFFFFFF          # 候选项底色
      # preedit_back_color:                   # 拼音底色 （inline_preedit: false）
      hilited_back_color: 0xD75A00            # 选中拼音底色 （inline_preedit: false）
      border_color: 0xFFFFFF                  # 外边框颜色

    macos_dark:
      name: "MacOS 深色／MacOS Dark"
      author: 小码哥
      font_face: "PingFangSC"          # 字体及大小
      font_point: 16
      label_font_face: "PingFangSC"    # 序号字体及大小
      label_font_point: 12
      comment_font_face: "PingFangSC"  # 注字体及大小
      comment_font_point: 16
      candidate_format: "%c\u2005%@\u2005" # 编号 %c 和候选词 %@ 前后的空间
      candidate_list_layout: linear   # 候选排布：层叠 stacked | 行 linear
      text_orientation: horizontal    # 行文向： 横 horizontal | 纵 vertical
      inline_preedit: true            # 拼音位于： 候选框 false | 行内 true
      translucency: false             # 磨砂： false | true
      mutual_exclusive: false         # 色不叠加： false | true
      border_height: 1                # 外边框 高
      border_width: 1                 # 外边框 宽
      corner_radius: 5                # 外边框 圆角半径
      hilited_corner_radius: 5       # 选中框 圆角半径
      surrounding_extra_expansion: 0 # 候选项背景相对大小？
      shadow_size: 0                 # 阴影大小
      line_spacing: 5                # 行间距
      base_offset: 0                 # 字基高
      alpha: 1                       # 透明度，0~1
      spacing: 10                    # 拼音与候选项之间的距离 （inline_preedit: false）
      color_space: srgb                       # 色彩空间： srgb | display_p3
      back_color: 0x1f1e2d                  # 底色
      hilited_candidate_back_color: 0xD75A00  # 选中底色
      label_color: 0x999999                   # 序号颜色
      hilited_candidate_label_color: 0xFFFFFF # 选中序号颜色
      candidate_text_color: 0xe9e9ea          # 文字颜色
      hilited_candidate_text_color: 0xFFFFFF  # 选中文字颜色
      comment_text_color: 0x999999            # 注颜色
      hilited_comment_text_color: 0x999999    # 选中注颜色
      text_color: 0x808080                    # 拼音颜色 （inline_preedit: false）
      hilited_text_color: 0xFFFFFF            # 选中拼音颜色 （inline_preedit: false）
      candidate_back_color: 0x1f1e2d          # 候选项底色
      # preedit_back_color:                   # 拼音底色 （inline_preedit: false）
      hilited_back_color: 0xD75A00            # 选中拼音底色 （inline_preedit: false）
      border_color: 0x050505                  # 外边框颜色

    wechat_light:
      name: "微信浅色／Wechat Light"
      author: 小码哥
      font_face: "PingFangSC"          # 字体及大小
      font_point: 16
      label_font_face: "PingFangSC"    # 序号字体及大小
      label_font_point: 13
      comment_font_face: "PingFangSC"  # 注字体及大小
      comment_font_point: 16
      candidate_format: "%c\u2005%@\u2005" # 编号 %c 和候选词 %@ 前后的空间
      candidate_list_layout: linear   # 候选排布：层叠 stacked | 行 linear
      text_orientation: horizontal    # 行文向： 横 horizontal | 纵 vertical
      inline_preedit: true            # 拼音位于： 候选框 false | 行内 true
      translucency: false             # 磨砂： false | true
      mutual_exclusive: false         # 色不叠加： false | true
      border_height: 1                # 外边框 高
      border_width: 1                 # 外边框 宽
      corner_radius: 5                # 外边框 圆角半径
      hilited_corner_radius: 5       # 选中框 圆角半径
      surrounding_extra_expansion: 0 # 候选项背景相对大小？
      shadow_size: 0                 # 阴影大小
      line_spacing: 5                # 行间距
      base_offset: 0                 # 字基高
      alpha: 1                       # 透明度，0~1
      spacing: 10                    # 拼音与候选项之间的距离 （inline_preedit: false）
      color_space: srgb                       # 色彩空间： srgb | display_p3
      back_color: 0xFFFFFF                    # 底色
      hilited_candidate_back_color: 0x79af22  # 选中底色
      label_color: 0x999999                   # 序号颜色
      hilited_candidate_label_color: 0xFFFFFF # 选中序号颜色
      candidate_text_color: 0x3c3c3c          # 文字颜色
      hilited_candidate_text_color: 0xFFFFFF  # 选中文字颜色
      comment_text_color: 0x999999            # 注颜色
      hilited_comment_text_color: 0x999999    # 选中注颜色
      text_color: 0x424242                    # 拼音颜色 （inline_preedit: false）
      hilited_text_color: 0x999999            # 选中拼音颜色 （inline_preedit: false）
      candidate_back_color: 0xFFFFFF          # 候选项底色
      # preedit_back_color:                   # 拼音底色 （inline_preedit: false）
      hilited_back_color: 0x79af22            # 选中拼音底色 （inline_preedit: false）
      border_color: 0xFFFFFF                  # 外边框颜色

    wechat_dark:
      name: "微信深色／Wechat Dark"
      author: 小码哥
      font_face: "PingFangSC"          # 字体及大小
      font_point: 16
      label_font_face: "PingFangSC"    # 序号字体及大小
      label_font_point: 13
      comment_font_face: "PingFangSC"  # 注字体及大小
      comment_font_point: 16
      candidate_format: "%c\u2005%@\u2005" # 编号 %c 和候选词 %@ 前后的空间
      candidate_list_layout: linear   # 候选排布：层叠 stacked | 行 linear
      text_orientation: horizontal    # 行文向： 横 horizontal | 纵 vertical
      inline_preedit: true            # 拼音位于： 候选框 false | 行内 true
      translucency: false             # 磨砂： false | true
      mutual_exclusive: false         # 色不叠加： false | true
      border_height: 1                # 外边框 高
      border_width: 1                 # 外边框 宽
      corner_radius: 5                # 外边框 圆角半径
      hilited_corner_radius: 5       # 选中框 圆角半径
      surrounding_extra_expansion: 0 # 候选项背景相对大小？
      shadow_size: 0                 # 阴影大小
      line_spacing: 5                # 行间距
      base_offset: 0                 # 字基高
      alpha: 1                       # 透明度，0~1
      spacing: 10                    # 拼音与候选项之间的距离 （inline_preedit: false）
      color_space: srgb                       # 色彩空间： srgb | display_p3
      back_color: 0x151515                    # 底色
      hilited_candidate_back_color: 0x79af22  # 选中底色
      label_color: 0x999999                   # 序号颜色
      hilited_candidate_label_color: 0xFFFFFF # 选中序号颜色
      candidate_text_color: 0xbbbbbb          # 文字颜色
      hilited_candidate_text_color: 0xFFFFFF  # 选中文字颜色
      comment_text_color: 0x999999            # 注颜色
      hilited_comment_text_color: 0xFFFFFF    # 选中注颜色
      text_color: 0xbbbbbb                    # 拼音颜色 （inline_preedit: false）
      hilited_text_color: 0x999999            # 选中拼音颜色 （inline_preedit: false）
      candidate_back_color: 0x151515          # 候选项底色
      # preedit_back_color:                   # 拼音底色 （inline_preedit: false）
      hilited_back_color: 0x79af22            # 选中拼音底色 （inline_preedit: false）
      border_color: 0x292929                  # 外边框颜色

```

```
# weasel.custom.yaml
patch:
  style:
    #color_scheme: ms_babyblue
    #color_scheme: ms_blue
    color_scheme: ms_wechatgreen
    font_face: "微软雅黑"
    font_point: 10
    horizontal: true
    inline_preedit: true
    label_font_point: 10
    layout:
      border: 2
      round_corner: 10
      margin_x: 0
      margin_y: 0
      hilite_padding: 15
      hilite_spacing: 5
      spacing: 0
      candidate_spacing: 0
  preset_color_schemes:
    ms_babyblue:
      name: "MS 宝宝蓝／MS BabyBlue"
      author: 小码哥
      candidate_format: "%c\u2005%@\u2005" # 编号 %c 和候选词 %@ 前后的空间
      back_color: 0xFFFFFF                    # 底色
      hilited_candidate_back_color: 0xFFD8A6  # 选中底色
      label_color: 0x3c3c3c                   # 序号颜色
      hilited_label_color: 0x3c3c3c           # 序号颜色
      hilited_candidate_label_color: 0x3c3c3c # 选中序号颜色
      candidate_text_color: 0x3c3c3c          # 文字颜色
      hilited_candidate_text_color: 0x000000  # 选中文字颜色
      comment_text_color: 0x999999            # 注颜色
      hilited_comment_text_color: 0xFFFFFF    # 选中注颜色
      text_color: 0x424242                    # 拼音颜色 （inline_preedit: false）
      hilited_text_color: 0xFFFFFF            # 选中拼音颜色 （inline_preedit: false）
      candidate_back_color: 0xFFFFFF          # 候选项底色
      # preedit_back_color:                   # 拼音底色 （inline_preedit: false）
      hilited_back_color: 0xD75A00            # 选中拼音底色 （inline_preedit: false）
      border_color: 0xeaeaea                  # 外边框颜色

    ms_blue:
      name: "MS 蓝色／MS Blue"
      author: 小码哥
      candidate_format: "%c\u2005%@\u2005" # 编号 %c 和候选词 %@ 前后的空间
      back_color: 0xFFFFFF                    # 底色
      hilited_candidate_back_color: 0xD75A00  # 选中底色
      label_color: 0x999999                   # 序号颜色
      hilited_label_color: 0xFFFFFF           # 序号颜色
      hilited_candidate_label_color: 0xFFFFFF # 选中序号颜色
      candidate_text_color: 0x3c3c3c          # 文字颜色
      hilited_candidate_text_color: 0xFFFFFF  # 选中文字颜色
      comment_text_color: 0x999999            # 注颜色
      hilited_comment_text_color: 0xFFFFFF    # 选中注颜色
      text_color: 0x424242                    # 拼音颜色 （inline_preedit: false）
      hilited_text_color: 0xFFFFFF            # 选中拼音颜色 （inline_preedit: false）
      candidate_back_color: 0xFFFFFF          # 候选项底色
      # preedit_back_color:                   # 拼音底色 （inline_preedit: false）
      hilited_back_color: 0xD75A00            # 选中拼音底色 （inline_preedit: false）
      border_color: 0xeaeaea                  # 外边框颜色

    ms_wechatgreen:
      name: "MS 微信绿／MS Wechat Green"
      author: 小码哥
      candidate_format: "%c\u2005%@\u2005" # 编号 %c 和候选词 %@ 前后的空间
      back_color: 0xFFFFFF                    # 底色
      hilited_candidate_back_color: 0x79af22  # 选中底色
      label_color: 0x999999                   # 序号颜色
      hilited_label_color: 0xFFFFFF           # 序号颜色
      hilited_candidate_label_color: 0xFFFFFF # 选中序号颜色
      candidate_text_color: 0x3c3c3c          # 文字颜色
      hilited_candidate_text_color: 0xFFFFFF  # 选中文字颜色
      comment_text_color: 0x999999            # 注颜色
      hilited_comment_text_color: 0xFFFFFF    # 选中注颜色
      text_color: 0x424242                    # 拼音颜色 （inline_preedit: false）
      hilited_text_color: 0xFFFFFF            # 选中拼音颜色 （inline_preedit: false）
      candidate_back_color: 0xFFFFFF          # 候选项底色
      # preedit_back_color:                   # 拼音底色 （inline_preedit: false）
      hilited_back_color: 0x79af22            # 选中拼音底色 （inline_preedit: false）
      border_color: 0xeaeaea                  # 外边框颜色
```

## 设置默认使用英文标点
关于这条, 很多人不喜欢中文下用英文标点符号, 请忽略, 我个人还是习惯这种, 打一个标点再打一个空格.

```
# double_pinyin_flypy.custom.yaml
patch:
  switches:
    - name: ascii_mode
      states: [ 中, A ]
      reset: 0
    - name: ascii_punct # 中英标点
      states: [ ¥, $ ]
      reset: 1
    - name: traditionalization
      states: [ 简, 繁 ]
      reset: 0
    - name: emoji
      states: [ 💀, 😄 ]
      reset: 1
    - name: full_shape
      states: [ 半角, 全角 ]
      reset: 0
```

要说一下半角和全角, 英文标点也是有半角和全角之分的, 所以要使用中英标点来区分.

## 设置常用自定义文本

```
# custom_phrase_double.txt
175xxxx0565	sj
37xxxxxxxxxxxxxxxx	sfz
faxxxxxxxh@gmail.com	yx
山东省青岛市xxxxxxxxxxxxxxx	dz

```

这样就和系统自带的 `Text Replacement` 功能一样了. 因为我用双拼, 需要在 `custom_phrase_double.txt` 里面创建, 而不是默认的 `custom_phrase.txt`.


## 自动更新词库与部署
虽然说 Rime 是一款功能齐全的输入法, 但如果没有词库, 还不如直接使用系统的输入法, 没有了词库便没有了灵魂, 搜狗输入法这种联网的会担心隐私问题, 所以 Rime + 词库能解决, 需要将词库下载到本地库中, 当然还有一些表情符号等.

输入法也有同步功能, 点一下同步, 会自动将配置文件全部同步到 `sync/YOUR_INSTALLATION_ID` 下面.

当然我们希望它能够自动更新, 所以可以使用下面这段脚本:

```
#!/bin/bash

LOGFILE=~/Library/Logs/update_rime_and_deploy.log
mkdir -p ~/Library/Logs

log() {
    level=$1
    shift
    msg="$@"
    date=$(date "+%Y-%m-%d %H:%M:%S")
    echo "[$date] [$level] $msg" >> "$LOGFILE"
}

{
    set -e

    cd ~/.rime/plum

    log "INFO" "Updating 㞢..."

    bash rime-install iDvel/rime-ice:others/recipes/all_dicts
    bash rime-install iDvel/rime-ice:others/recipes/opencc

    sleep 3

    log "INFO" "Syncing 㞢..."
    /Library/Input\ Methods/Squirrel.app/Contents/MacOS/Squirrel --sync

    log "INFO" "Deploying 㞢..."
    /Library/Input\ Methods/Squirrel.app/Contents/MacOS/Squirrel --reload

    osascript -e 'display notification "Rime deployment succeeded 🍻" with title "Plum Update"'

    log "INFO" "Rime deployment succeeded"
} 2>&1

```

这段脚本保存在 `~/.rime/update_rime_and_deploy.sh` 中, 然后新建一个 `~/Library/LaunchAgents/com.faichou.rime.plist`:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
        "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>com.faichou.rime</string>
  <key>ProgramArguments</key>
  <array>
    <string>/bin/sh</string>
    <string>/Users/<Username>/.rime/update_rime_and_deploy.sh</string>
  </array>
  <key>StartCalendarInterval</key>
  <dict>
    <key>Hour</key>
    <integer>12</integer>
    <key>Minute</key>
    <integer>0</integer>
  </dict>
</dict>
</plist>

```

命令执行:

```
chmod +x ~/.rime/update_rime_and_deploy.sh

sudo launchctl bootstrap system /Library/LaunchDaemons/com.faichou.rime.plist

```
