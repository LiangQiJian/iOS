# iOS
Circular progress bar for iOS
using System;
using UIKit;
using CoreGraphics;
using Foundation;

namespace Canvas
{
	public class DrawingView:UIView
	{
		//进度的百分比
		private int percent;
		public DrawingView(CGRect frame):base(frame)
		{
			percent = 0;
		}
		public override void Draw(CGRect rect)
		{
			base.Draw(rect);
			//获得当前画板
			CGContext context = UIGraphics.GetCurrentContext();
			//画进度
			context.SetLineWidth(4f);
			context.SetStrokeColor(UIColor.DarkGray.CGColor);
			context.AddArc(rect.Width/2, rect.Height/2, rect.Height/2-5,(float)(Math.PI)*3/2 ,(float)(Math.PI/50 * percent)+(float)(Math.PI)*3/2, false);
			context.StrokePath();
			//添加文字
			UIFont font = UIFont.FromName("Verdana-Bold", 12f);
			NSString drawText = new NSString("下载"+percent.ToString()+"%");
			//获得文字的高度与宽度
			CGSize textSize = CalculateTextSize(font, drawText);
			context.SetStrokeColor(UIColor.Green.CGColor);
			context.SetLineWidth(4f);
			//通过自适应高度与宽度，让文字居中
			drawText.DrawString(new CGPoint((rect.Width-textSize.Width)/2, rect.Height/2-textSize.Height/2), font);
			context.SetTextDrawingMode(CGTextDrawingMode.StrokeClip);
		}
		public void StartProgress(int _precent)
		{
			percent = _precent;
			//重新规划画板
			SetNeedsDisplayInRect(this.Bounds);
		}
		//获得文字的高度与宽度
		CGSize CalculateTextSize(UIFont font, NSString text)
		{
			CGSize texeSize;
			texeSize = text.StringSize(font);
			return texeSize;
		}
	}
}
