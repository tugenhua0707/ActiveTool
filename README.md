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
 
        
        var ActiveTool = (function(win,undefined){
        
	     var doc = document,
	   
		 docBody = doc.body,
		
		 isIE = navigator.userAgent.match(/MSIE/)!= null,
		
		 isIE6 = navigator.userAgent.match(/MSIE 6.0/)!= null,
		
		 docelem = doc.documentElement || docBody;

	    function ActiveTool(container,config){
	
		var self = this;
		
		if(!(self instanceof ActiveTool)){
		
			return new ActiveTool(container,config);
			
		}
		
		config = $.extend(ActiveTool.Config,config);

		self.config = config || {};
		
		// 匹配传参container
		if($.isPlainObject(container)){
			self.container = container;
		}else if(/^\./.test(container)){
			self.container = $(container);
		}else if(/^#/.test(container)){
			self.container = $(container);
		}else if($('.'+container)){
			self.container = $('.'+container);
		}else{
			throw new Error('点击元素传递参数不符合!');
		}
		
		self._init();
		
	}
	
	// 默认配置

	ActiveTool.Config = {
	
		topLink: '.top',
		
		markupType: 0,
		
		contentWidth: 1000,
		
		paddingWidth: 5,
		
		leftOffset: 0,
		
		left:0,
		
		right:0,
		
		YAlign: {
			top:0
		},
		
		zIndex:99999,
		
		display: 0
		
	};
	
	ActiveTool.prototype = {
	
		_init: function(){
			var self = this,
			
			    cfg = self.config,
			    
			    EVT_SCROLL = 'scroll',
			    
			    EVT_RESIZE = 'resize',
			    
			    divBar;
			    
			// init divBar node
			divBar = self.container;
			
			// init position
			self._calcPos();

			// 首屏是否显示
			if(cfg.display === 0){
				$(divBar).css({"opacity":0})
				$(divBar).css('zIndex',-1);
			}else{
				$(divBar).css({"opacity":1});
				$(divBar).css('zIndex',cfg.zIndex);

			}

			var topLink = $(cfg.topLink,divBar);
			
			if (topLink){
				// hide focus for ie
				if (isIE) {
					topLink.hideFocus = true;
				}
				$(topLink).bind('click',function(e){
					e.preventDefault();
					$(topLink).unbind('click');
					docelem.scrollTop = 0;
				});
			}
			
			// do decoration on scrolling
			
			$(win).bind(EVT_SCROLL,function(e){
				self._decorate(true);
			});

			$(win).bind(EVT_RESIZE,function(e){
				self._decorate(false);
			});
		},
		//计算各浏览下的定位坐标
		_calcPos: function(){
			var self = this, 
				cfg = self.config,
				PX = 'px',
				TOP = 'top',
				BOTTOM = 'bottom',
				LEFT = 'left',
				RIGHT = 'right',
				contentWidth = cfg.contentWidth,
				paddingWidth = cfg.paddingWidth,
				leftOffset = cfg.leftOffset,
				divBar = self.container,
				top = cfg.YAlign.top,
				bottomPadding = parseInt(cfg.YAlign.bottom,10),
				topPadding = parseInt(top,10),
				selfHeight = parseInt($(divBar).css('height'),10),//divbar自身高度
				selfWidth = parseInt($(divBar).css('width'),10),//divbar自身宽度
				xPos,
				checkMarkup = function(viewWidth){
					switch (true){

						case cfg.markupType === 0:
							xPos = Math.floor((viewWidth - contentWidth) / 2) - leftOffset;
							break;

						case cfg.markupType === 2:
							xPos = Math.floor((viewWidth - contentWidth) / 2) + contentWidth + paddingWidth;
							break;

						case cfg.markupType === -2:
							xPos = parseInt(cfg.right,10);
							$(divBar).css(RIGHT, xPos + PX);
							break;

						case cfg.markupType === 1:
							xPos = Math.floor((viewWidth - contentWidth) / 2) - paddingWidth - selfWidth;
							break;

						case cfg.markupType === -1:
							xPos = parseInt(cfg.left,10);
							break;

						default:
							xPos = Math.floor((viewWidth - contentWidth) / 2) - leftOffset;
					}
				},
				cal;
			if (true === isIE6) {
				// for IE6
				cal = function(scrolling){
					var viewHeight = $(win).height(),
						scrollTop = docelem.scrollTop,
						yPos;

					if (top !== undefined){
						yPos = topPadding + scrollTop;
					} else {
						yPos = viewHeight - bottomPadding - selfHeight + scrollTop;
					}

					// on scrolling
					if (scrolling) {
						DOM.css(divBar, TOP, yPos + PX);
						return;
					}

					var viewWidth = $(win).width();
					checkMarkup(viewWidth);
					$(divBar).css({'position':'absolute'});
					$(divBar).css(TOP, yPos + PX);
					if (cfg.markupType === -2){return;}
					$(divBar).css(LEFT, xPos + PX);
				};
			} else if (isIE) {
				// for IE7+ （ie9有待验证）
				cal = function () {
					$(divBar).css({'position':'fixed'});
					top !== undefined ? $(divBar).css(TOP, topPadding + PX) : $(divBar).css(BOTTOM, bottomPadding + PX);
					var viewWidth = $(win).width();
					checkMarkup(viewWidth);
					if (cfg.markupType === -2){return;}
					$(divBar).css(LEFT, xPos + PX);
				};
			} else {
				// for non-IE
				cal = function () {
					
					$(divBar).css({'position':'fixed'});
					top !== undefined ? $(divBar).css(TOP, topPadding + PX) : $(divBar).css(BOTTOM, bottomPadding + PX);
					var viewWidth = document.body.clientWidth;
					checkMarkup(viewWidth);
					if (cfg.markupType === -2){return;}
					$(divBar).css(LEFT, xPos + PX);
				};
			}
			self._calcPos = cal;
			return cal();
		},
		/* win 注册 scroll resize 事件
		 * @param flag -> true 指滚动 flag -> false 指缩放 
		 */
		_decorate: function(flag){
			var self = this,
				cfg = self.config,
				divBar = self.container,
				DELAY = 100,
				scrollTimer,
				resizeTimer;
			if(flag){
				var scrollTop = document.documentElement.scrollTop + document.body.scrollTop;
				if(0 === scrollTop && cfg.display === 0){
					$(divBar).css({'opacity':0});
					$(divBar).css('zIndex', -1);
				}else {
					$(divBar).css({'opacity':1});
					$(divBar).css('zIndex', cfg.zIndex);
					scrollTimer && clearTimeout(scrollTimer);
					scrollTimer = setTimeout(function(){
						self._calcPos();
					},DELAY);
				}
			}else{
				resizeTimer && clearTimeout(resizeTimer);
				resizeTimer = setTimeout(function(){
					self._calcPos();
				},DELAY);
			}
		}
	};

	return ActiveTool;
	
})(window);


页面HTML代码如下：

   <div style="width:100%;">
   
   	<div style="height:2000px;"></div>
   	
        <div class="go-top" id="J_gotop">
        
		<a title="返回顶部" href="#" class="gotop" data-spm-anchor-id="0.0.0.0">返回顶部</a>
		
	</div> 
	
   </div>
   
javascript调用方式如下：

	new ActiveTool('#J_gotop',{topLink:'.gotop',markupType:2,YAlign: {
		bottom:200
	}});
  
