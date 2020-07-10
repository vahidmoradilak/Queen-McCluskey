//# Queen-McCluskey
#include <iostream>
//#include <conio.h>
 using namespace std;
 
 //---------------------------------------------------- p0 --------------------//
 struct p0
 {
 	int v=-1;
 	bool w=false;
 	int l=-1;
 	void print(bool flag)
 	{
 		string st="";
 		string s[5]={"a'", "b'", "c'", "d'", "e'"};
 		if(flag)
 		{
 			if(v & 16)   s[0]='a'; st+=s[0];
 			if(v & 8)     s[1]='b'; st+=s[1];
 			if(v & 4)     s[2]='c'; st+=s[2];
 			if(v & 2)     s[3]='d'; st+=s[3];
 			if(v & 1)     s[4]='e'; st+=s[4];
 		}
 		else
 		{
 			if(!(v & 16))   s[0]='a'; st+=s[0];
 			if(!(v & 8))     s[1]='b'; st+='+' + s[1];
 			if(!(v & 4))     s[2]='c'; st+='+' + s[2];
 			if(!(v & 2))     s[3]='d'; st+='+' + s[3];
 			if(!(v & 1))     s[4]='e'; st+='+' + s[4];
 		}
 		cout<<st;
 	}
 };
  //---------------------------------------------------- p1 --------------------//
 struct p1
 {
 	int a[2]={-1};
 	int l=-1;
 	int d=-1;
 	bool w=false;
 };
  //---------------------------------------------------- p2 --------------------//
 struct p2
 {
 	int a[4]={-1};
 	int l=-1;
 	int d[3]={-1};
 	bool w=false;
 };
  //---------------------------------------------------- p3 --------------------//
 struct p3
 {
 	int a[8]={-1};
 	int l=-1;
 	int d[7]={-1};
 	bool w=false;
 };
 //---------------------------------------------------- p4 --------------------//
 struct p4
 {
 	int a[16]={-1};
 	int l=-1;
 	int d[15]={-1};
 	bool w=false;
 };
 //---------------------------------------------------- p5 --------------------//
 struct p5
 { bool full=false; };
 
//---------------------------------------------------- name --------------------//
 void meno();
 void piAndEpi(bool);
 int findLevel(int);
 int findExp(int);
 void complete0(p0*, int*, int, int, bool);
 void complete1(p0*, p1*, int, int);
 void complete2(p1*, p2*);
 void complete3(p2*, p3*);
 void complete4(p3*, p4*);
 void complete5(p4*, p5);
 template<class T>
 void setV(T*, int*, int, int, int);
 template<class T>
 void setD(T*, int*, int, int);
 template<class T>
 bool distant(T*, int, int, int);
 template<class T>
 void print(T&, int, bool);
 bool findEpi(int**, int, int, int*, int);
 bool dCare(int, int*, int);
//=========================  main  ===========//
int main()
{
	meno();
//	getche();
	return 0;
}
//=========================  tavabe  ===========//

//---------------------------------------------------- meno --------------------//
void meno()
{
	cout<<"             welcome\n";
	char key=1;
	while(key!='3')
	{
		cout<<"Please Enter One Number\n"
		      "   1)CSOP & don`t care\n"
		      "   2)CPOS & don`t care\n   3)Finish   --->   ";
		cin>>key, cin.get();
		system("cls");
		 
		if(key=='1')
			piAndEpi(true);
		else if(key=='2')
			piAndEpi(false);
		else if(key=='3')
			cout<<".End.";
		else
			cout<<"!!!    EROR: number is false.\n";
	}
}

//---------------------------------------------------- piAndEpi --------------------//
void piAndEpi(bool flag)
{
	if(flag)
		cout<<"Enter Number Minterms & don`t care\n"
	      	"   Exampel (17 6)   : ";
	else
		cout<<"Enter Number Maxterms & don`t care\n"
	      	"   Exampel (17 6)   : ";
	int x,y;
	cin>>x>>y;
	int dc[y];
	p0 a[x+y];
	p1 b[80];
	p2 c[80];
	p3 d[40];
	p4 e[10];
	p5 f;
	complete0(a, dc, x, y, flag);
	complete1(a, b, x, y);
	complete2(b, c);
	complete3(c, d);
	complete4(d, e);
	complete5(e, f);
	int pi[16][2];
	int** epi=new int* [16];
	for(int i=0; i<16; i++)
		epi[i]=new int [19], epi[i][17]=0;
	int ipi=0;
	for(int i=0; i<x+y; i++)										// pi level 0
		if(a[i].w==0)
			epi[ipi][0]=1, epi[ipi][18]=i, epi[ipi][1]=a[i].v,
			pi[ipi][0]=0, pi[ipi++][1]=i;
	for(int i=0; b[i].a[0]!=-1; i++)						// pi level 1
		if(b[i].w==0)
		{
			for(int j=1; j<=2; epi[ipi][j]=b[i].a[j-1], j++);
			epi[ipi][0]=2, epi[ipi][18]=i, pi[ipi][0]=1, pi[ipi++][1]=i;
		}
	for(int i=0; c[i].a[0]!=-1; i++)						// pi level 2
		if(c[i].w==0)
		{
			for(int j=1; j<=4; epi[ipi][j]=c[i].a[j-1], j++);
			epi[ipi][0]=4, epi[ipi][18]=i, pi[ipi][0]=2, pi[ipi++][1]=i;
		}
	for(int i=0; d[i].a[0]!=-1; i++)						// pi level 3
		if(d[i].w==0)
		{
			for(int j=1; j<=8; epi[ipi][j]=d[i].a[j-1], j++);
			epi[ipi][0]=8, epi[ipi][18]=i, pi[ipi][0]=3, pi[ipi++][1]=i;
		}
	for(int i=0; e[i].a[0]!=-1; i++)						// pi level 4
		if(e[i].w==0)
		{
			for(int j=1; j<=16; epi[ipi][j]=e[i].a[j-1], j++);
			epi[ipi][0]=16, epi[ipi][18]=i, pi[ipi][0]=4, pi[ipi++][1]=i;
		}
	if(f.full==true)
	{
		cout<<"number pi=1\nnumber epi=1\nf=1";
		return;
	}
	system("cls");
	cout<<"number pi="<<ipi<<endl;
	for(int i=0; i<ipi; cout<<endl, i++)
	{
		if(pi[i][0]==0)
			a[pi[i][1]].print(flag);
		else if(pi[i][0]==1)
			print(b[pi[i][1]], 1, flag);
		else if(pi[i][0]==2)
			print(c[pi[i][1]], 3, flag);
		else if(pi[i][0]==3)
			print(d[pi[i][1]], 7, flag);
		else
			print(e[pi[i][1]], 15, flag);
	}
	cout<<"\n\n";
	
	
	int iepi=0;
	for(int i=0; i<ipi; i++)
		if(findEpi(epi, ipi, i, dc, y))
			iepi++;
	cout<<"number epi="<<iepi<<endl;
	for(int i=0; i<ipi; i++)
	{
		bool out=1;
		if(epi[i][17] && epi[i][0]==1)
			a[epi[i][18]].print(flag);
		else if(epi[i][17] && epi[i][0]==2)
			print(b[epi[i][18]], 1, flag);
		else if(epi[i][17] && epi[i][0]==4)
			print(c[epi[i][18]], 3, flag);
		else if(epi[i][17] && epi[i][0]==8)
			print(d[epi[i][18]], 7, flag);
		else if(epi[i][17] && epi[i][0]==16)
			print(e[epi[i][18]], 15, flag);
		else
			out=0;
		if(out)
			cout<<endl;
	}
	cout<<"\n\n";
	
	for(int i=0; i<16; i++)
		delete [] epi[i];
	delete [] epi;
}

//---------------------------------------------------- findLevel --------------------//
int findLevel(int x)
{
	int s=0;
	for(int i=1<<15; i>0; i=i>>1)
		if(i & x)
			s++;
	return s;
}
//---------------------------------------------------- findExp --------------------//
int findExp(int x)
{
	int s=0;
	for(int i=1<<15; i>0; i=i>>1)
		if(i == x)
			s++;
	return s;
}

//---------------------------------------------------- complete0 --------------------//
void complete0(p0* a, int* dc, int x, int y, bool flag)
{
	for(int i=0; i<x+y; i++)
	{
		if(i<x && flag)
			cout<<"Enter Minterm    ( "<<i+1<<" ) --> ";
		else if(i<x)
			cout<<"Enter Maxterm    ( "<<i+1<<" ) --> ";
		else
			cout<<"Enter don`t care ( "<<i-x+1<<" ) --> ";
		cin>>a[i].v;
		if(i>=x)
			dc[i-x]=a[i].v;
		a[i].l=findLevel(a[i].v);
	}
}

//---------------------------------------------------- complete1 --------------------//
void complete1(p0* a, p1* b, int x, int y)
{
	for(int i=0; i<x+y; i++)
		for(int j=0; j<x+y; j++)
			if(a[i].l +1 == a[j].l && findExp(a[j].v - a[i].v))
				{
					a[i].w=1, a[j].w=1;
					int k;
					for(k=0; b[k].a[0]!=-1; k++);
					int s=0;
					for(int z=0; z<k; z++)
						if(b[z].a[0]==a[i].v && b[z].a[1]==a[j].v)
							s=1, z=k;	//break
					if(s==0)
						b[k].a[0]=a[i].v, b[k].a[1]=a[j].v,
						b[k].l=a[i].l, b[k].d=a[j].v - a[i].v;
				}
}

//---------------------------------------------------- complete2 --------------------//
void complete2(p1* b, p2* c)
{
	for(int i=0; b[i].a[0]!=-1; i++)
		for(int j=0; b[j].a[0]!=-1; j++)
			if(b[i].l+1==b[j].l && b[i].d==b[j].d && findExp(b[j].a[0] - b[i].a[0]))
			{
				b[i].w=1, b[j].w=1;
				int k;
				for(k=0; c[k].a[0]!=-1; k++);
				int s=0;
				int v[4];
				setV(b, v, i, j, 4);
				for(int z=0; z<k; z++)
					if(c[z].a[0]==v[0] && c[z].a[1]==v[1] &&
					c[z].a[2]==v[2] && c[z].a[3]==v[3])
					s=1, z=k;	//break
				if(s==0)
					c[k].a[0]=v[0], c[k].a[1]=v[1], c[k].a[2]=v[2], c[k].a[3]=v[3],
					c[k].l=b[i].l, setD(c, v, k, 3);
			}
}

//---------------------------------------------------- complete3 --------------------//
void complete3(p2* c, p3* d)
{
	for(int i=0; c[i].a[0]!=-1; i++)
		for(int j=0; c[j].a[0]!=-1; j++)
			if(c[i].l+1==c[j].l && distant(c,i,j,3) && findExp(c[j].a[0]- c[i].a[0]))
			{
				c[i].w=1, c[j].w=1;
				int k;
				for(k=0; d[k].a[0]!=-1; k++);
				int s=0;
				int v[8];
				setV(c, v, i, j, 8);
				for(int z=0; z<k; z++)
					if(d[z].a[0]==v[0] && d[z].a[1]==v[1] &&
						d[z].a[2]==v[2] && d[z].a[3]==v[3] &&
						d[z].a[4]==v[4] && d[z].a[5]==v[5] &&
						d[z].a[6]==v[6] && d[z].a[7]==v[7])
						s=1, z=k;	//break
				if(s==0)
					d[k].a[0]=v[0], d[k].a[1]=v[1], d[k].a[2]=v[2], d[k].a[3]=v[3],
					d[k].a[4]=v[4], d[k].a[5]=v[5], d[k].a[6]=v[6], d[k].a[7]=v[7],
					d[k].l=c[i].l, setD(d, v, k, 7);
			}
}

//---------------------------------------------------- complete4 --------------------//
void complete4(p3* d, p4* e)
{
	for(int i=0; d[i].a[0]!=-1; i++)
		for(int j=0; d[j].a[0]!=-1; j++)
			if(d[i].l+1==d[j].l && distant(d,i,j,7) && findExp(d[j].a[0]-d[i].a[0]))
			{
				d[i].w=1, d[j].w=1;
				int k;
				for(k=0; e[k].a[0]!=-1; k++);
				int s=0;
				int v[16];
				setV(d, v, i, j, 16);
				for(int z=0; z<k; z++)
					if(e[z].a[0]==v[0] && e[z].a[1]==v[1] &&
						e[z].a[2]==v[2] && e[z].a[3]==v[3] &&
						e[z].a[4]==v[4] && e[z].a[5]==v[5] &&
						e[z].a[6]==v[6] && e[z].a[7]==v[7] &&
						e[z].a[8]==v[8] && e[z].a[9]==v[9] &&
						e[z].a[10]==v[10] && e[z].a[11]==v[11] &&
						e[z].a[12]==v[12] && e[z].a[13]==v[13] &&
						e[z].a[14]==v[14] && e[z].a[15]==v[15])
						s=1, z=k;	//break
				if(s==0)
					e[k].a[0]=v[0], e[k].a[1]=v[1], e[k].a[2]=v[2], e[k].a[3]=v[3],
					e[k].a[4]=v[4], e[k].a[5]=v[5], e[k].a[6]=v[6], e[k].a[7]=v[7],
					e[k].a[8]=v[8], e[k].a[9]=v[9], e[k].a[10]=v[10],
					e[k].a[11]=v[11], e[k].a[12]=v[12], e[k].a[13]=v[13],
					e[k].a[14]=v[14], e[k].a[15]=v[15], e[k].l=d[i].l, setD(e,v,k,15);
			}
}

//---------------------------------------------------- complete5 --------------------//
void complete5(p4* e, p5 f)
{
	for(int i=0; e[i].a[0]!=-1; i++)
		for(int j=0; e[j].a[0]!=-1; j++)
			if(e[i].l+1==e[j].l && distant(e,i,j,15)&& findExp(e[j].a[0]-e[i].a[0]))
				e[i].w=1, e[j].w=1, f.full=1;
}

//---------------------------------------------------- setV --------------------//
template<class T>
void setV(T* t, int* v, int x, int y, int n)
{
	for(int i=0; i<n; i++)
		if(i<n/2)
			v[i]=t[x].a[i];
		else
			v[i]=t[y].a[i-n/2];
	for(int i=1, j=1; j<n; i=++j)
	{
		int k;
		for(k=v[j]; i-- && k<v[i]; v[i+1]=v[i]);
		v[i+1]=k;
	}
}

//---------------------------------------------------- setD --------------------//
template<class T>
void setD(T* t, int* v, int k, int n)
{
	for(int i=0; i<n; t[k].d[i]=v[i+1] - v[i], i++);
}

//---------------------------------------------------- distant --------------------//
template<class T>
bool distant(T* t, int x, int y, int n)
{
	bool s=1;
	for(int i=0; i<n; i++)
		if(t[x].d[i]!=t[y].d[i])
			s=0, i=n;	//break
	return s;
}

//---------------------------------------------------- print --------------------//
template<class T>
void print(T& p, int n, bool flag)
{
	int x=p.a[0];
	int y=p.a[n] - x;
	string s[5]={"a'", "b'", "c'", "d'", "e'"};
	string st="";
	if(flag)
	{
		if(!(y & 16))   { if(x & 16)   s[0]='a'; st+=s[0]; }
		if(!(y & 8))     { if(x & 8)      s[1]='b'; st+=s[1]; }
		if(!(y & 4))     { if(x & 4)      s[2]='c'; st+=s[2]; }
		if(!(y & 2))     { if(x & 2)      s[3]='d'; st+=s[3]; }
		if(!(y & 1))     { if(x & 1)      s[4]='e'; st+=s[4]; }
	}
	else
	{
		int plus=0;
		if(!(y & 16))   { if(!(x & 16))   s[0]='a'; st+=s[0], plus=1; }
		if(!(y & 8))     { if(!(x & 8))      s[1]='b'; if(plus++) st+='+'; st+=s[1]; }
		if(!(y & 4))     { if(!(x & 4))      s[2]='c'; if(plus++) st+='+'; st+=s[2]; }
		if(!(y & 2))     { if(!(x & 2))      s[3]='d'; if(plus++) st+='+'; st+=s[3]; }
		if(!(y & 1))     { if(!(x & 1))      s[4]='e'; if(plus++) st+='+'; st+=s[4]; }
	}
	cout<<st;
}

//---------------------------------------------------- findEpi --------------------//
bool findEpi(int** epi,int ipi, int i, int* dc, int y)
{
	for(int j=0; j<epi[i][0]; j++)
		if(!dCare(epi[i][j+1], dc, y))
		{
			int s=1;
			for(int k=0; k<ipi; k++)
				if(i!=k)
					for(int z=0; z<epi[k][0]; z++)
						if( !dCare(epi[k][z+1], dc, y) && epi[i][j+1] == epi[k][z+1] )
							s=0;
			if(s)
			{
				epi[i][17]=1;
				return 1;
			}
		}
	return 0;
}

//---------------------------------------------------- dCare --------------------//
bool dCare(int x, int* d, int y)
{
	int s=0;
	for(int i=0; i<y; i++)
		if(x==d[i])
			s=1, i=y;
	return s;
}
