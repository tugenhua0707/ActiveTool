ActiveTool
==========

                                                      jquery浮动条组件


 1. Setting:
 2. !!! css中需设置{width:**;bottom:**;}，否则后果自负
 3.
 4. 默认参数：
	        topLink:'.top', //toplink.

		markupType: 0, //默认为0：居中，1：居左，2：居右
		
		contentWidth: 1000, //布局宽度
		
		paddingWidth: 5, //仅当居右(左)时生效，表示距布局右(左)侧的间隔;
		
		leftOffset: 0, //仅当居中时生效，divbar左侧 需要超出布局的宽度
		
		//left和right 仅在markupType为-1 和 -2时有用。表示距窗口最左和最右的间隔
		
		left:0,
		
		right:0,
		
		//！！！top和bottom 必须设置其一 默认为top:0,顶对齐。
		YAlign: {
			top:0
		},
		
		zIndex:99999,//z-index属性
		
		display: 0//默认首屏不显示，为1则首屏显示。



 5. INSTANCE：
		//水平居中，顶部(间隔用YAlign: {top:0}设置,默认为0)
		new ActiveTool('#divbar',{
			topLink:'.top',//class or id
			markupType: 0
		});
		//水平居中，底部(间隔用YAlign: {bottom:'10px'}设置)
		new ActiveTool('#divbar',{
			topLink:'.top',//class or id
			markupType: 0,
			YAlign: {
				bottom:0
			}
		});
		//水平居左靠近主体左侧，顶部(间隔用YAlign: {top:0}设置)
		new ActiveTool('#divbar',{
			topLink:'.top',//class or id
			markupType: 1
		});
		//水平居左靠近主体左侧，底部(间隔用YAlign: {bottom:'10px'}设置)
		new ActiveTool('#divbar',{
			topLink:'.top',//class or id
			markupType: 1,
			YAlign: {
				bottom:0
			}
		});
		//水平居左，顶部(间隔用YAlign: {top:0}设置),左侧距离用style
		new ActiveTool('#divbar',{
			topLink:'.top',//class or id
			markupType: -1,
			left:'5px'
		});
		//水平居左，底部(间隔用YAlign: {bottom:'10px'}设置),左侧距离用left
		new ActiveTool('#divbar',{
			topLink:'.top',//class or id
			markupType: -1,
			left:'5px',
			YAlign: {
				bottom:0
			}
		});

	****水平居右与水平居左类似****
	
        2013-8-20 tugenhua@126.com
 

