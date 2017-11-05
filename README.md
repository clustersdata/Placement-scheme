# Placement-scheme

Placement scheme

	在n*n的正方形中放置长为2,宽为1的长条块,问放置方案如何
  
【参考程序１】

const n=4;

var  k,u,v,result:integer;

     a:array[1..n,1..n]of char;
     
procedure printf; {输出}

  begin
  
    result:=result+1;	{方案总数加1}
    
    writeln('--- ',result,' ---');
    
    for v:=1 to n do   begin
    
	for u:=1 to n do write(a[u,v]); writeln end; writeln;
  
  end;
  

procedure try;	 {填放长条块}

  var	 i,j,x,y:integer;  full:boolean;
  
  begin
  
    full:=true;
    
    if k<>trunc(n*n/2) then full:=false;{测试是否已放满}
    
    if full then printf;   {放满则可输出}
    
    if not full then  begin    {未满}
    
	 x:=0;y:=1;   {以下先搜索未放置的第一个空位置}
   
	 repeat
   
	   x:=x+1;
     
	   if x>n then begin x:=1;y:=y+1 end
     
	 until a[x,y]=' ';
   
   
    {找到后,分两种情况讨论}
    
	 if a[x+1,y]=' ' then   begin   {第一种情况:横向放置长条块}
   
	     k:=k+1;			{记录已放的长条数}
       
	     a[x,y]:=chr(k+ord('@'));   {放置}
       
	     a[x+1,y]:=chr(k+ord('@'));
       
	     try;			{递归找下一个空位置放}
       
	     k:=k-1;
       
	     a[x,y]:=' ';               {回溯,恢复原状}
       
	     a[x+1,y]:=' '
       
	 end;
   
	 if a[x,y+1]=' ' then   begin    {第二种情况:竖向放置长条块}
   
	     k:=k+1;			 {记录已放的长条数}
       
	     a[x,y]:=chr(k+ord('0'));    {放置}
       
	     a[x,y+1]:=chr(k+ord('0'));
       
	     try;			 {递归找下一个空位置放}
       
	     k:=k-1;
       
	     a[x,y]:=' ';                {回溯,恢复原状}
       
	     a[x,y+1]:=' '
       
	 end;
   
    end;
    
  end;
  
begin	 {主程序}

  fillchar(a,sizeof(a),' ');  {记录放置情况的字符数组,初始值为空格}
  
  result:=0; k:=0;  {k记录已放的块数,如果k=n*n/2,则说明已放满}
  
  try;	 {每找到一个空位置,把长条块分别横放和竖放试验}
  
end.


【参考程序２】

const dai:array [1..2,1..2]of integer=((0,1),(1,0));

type node=record

		w,f:integer;
    
		end;
    
var a:array[1..20,1..20]of integer;

path:array[0..200]of node;

s,m,n,nn,i,j,x,y,dx,dy,dep:integer;

p,px:boolean;

procedure inputn;

begin


{ write('input n');readln(n);}

 n:=4;
 
 nn:=n*n;m:=nn div 2;
 
end;

procedure print;

var i,j:integer;

begin

 inc(s);writeln('no',s);
 
 for i:=1 to n do begin
 
   for j:=1 to n do
   
     write(a[i,j]:3);writeln;
     
     end;
     
     writeln;
     
end;

function fg(h,v:integer):boolean;

var p:boolean;

begin

   p:=false;
   
   if (h<=n) and (v<=n) then
   
			if a[h,v]=0 then p:=true;
      
   fg:=p;
   
  end;
  
procedure back;

 begin
 
   dep:=dep-1;
   
   if dep=0 then begin p:=true ;px:=true;end
   
      else begin
      
      
	i:=path[dep].w;j:=path[dep].f;
  
	x:=((i-1)div n )+1;y:=i mod n;
  
	if y=0 then y:=n;
  
	dx:=x+dai[j,1];dy:=y+dai[j,2];
  
	a[x,y]:=0;a[dx,dy]:=0;
  
	end;
  
end;

begin

 inputn;
 
 s:=0;
 
 fillchar(a,sizeof(a),0);
 
 x:=0;y:=0;dep:=0;
 
 path[0].w:=0;path[0].f:=0;
 
 repeat
 
   dep:=dep+1;
   
   i:=path[dep-1].w;
   
   repeat
   
    i:=i+1;x:=((i-1)div n)+1;
    
    y:=i mod n;if y=0 then y:=n;
    
    px:=false;
    
    if fg(x,y)
    
     then begin
     
     j:=0;p:=false;
     
     repeat
     
     inc(j);
     
     dx:=x+dai[j,1];dy:=y+dai[j,2];
     
     if fg(dx,dy) and (j<=2) then begin
     
     
       a[x,y]:=dep;a[dx,dy]:=dep;
       
       path[dep].w:=i;path[dep].f:=j;
       
       if dep=m then begin print;dep:=m+1;back;end
       
		 else begin p:=true;px:=true;end;
     
		 end
     
       else if j>=2 then back
       
		    else p:=false;
        
       until p;
       
       end
       
       else if i>=nn then back
       
		  else px:=false;
      
     until px;
     
    until dep=0;
    
    readln;
    
 end.
