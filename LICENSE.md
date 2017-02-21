#include <iostream>
#include <cstdio>
#include <cstring>
using namespace std;
#define maxn 10000005
int sum,data[1005];
struct node
{
    int l,r,v;
}tree[2*maxn];
void build(int i,int l,int r)
{
    tree[i].l=l;
    tree[i].r=r;
    tree[i].v=0;
    if(l==r)
    {
        return ;
    }
    int mid=(l+r)/2;
    build(i*2,l,mid);
    build(i*2+1,mid+1,r);
}
void update(int id,int l,int r,int val)
{
    if(tree[id].l>r||tree[id].r<l)
    return ;
    if(tree[id].l==tree[id].r&&tree[id].l>=l&&tree[id].l<=r)
    {
        tree[id].v=val;
        return ;
    }
    int mid=(tree[id].l+tree[id].r)/2;
    if(mid>=r)
    update(id*2,l,r,val);
    else if(mid<l)
    update(id*2+1,l,r,val);
    else
    {
        update(id*2,l,mid,val);
        update(id*2+1,mid+1,r,val);
    }
}
void quest(int i)
{
    if(tree[i].l==tree[i].r)
    {
        if(tree[i].v!=0&&data[tree[i].v]==0)
        {
            sum++;
            data[tree[i].v]=1;
        }
        return ;
    }
    quest(i*2);
    quest(i*2+1);
}
int main()
{
    int n,x,a,b;
    scanf("%d",&n);
    while(n--)
    {
        build(1,1,maxn);
        scanf("%d",&x);
        for(int i=1;i<=x;i++)
        {
            scanf("%d%d",&a,&b);
            update(1,a,b,i);
        }
        sum=0;
        memset(data,0,sizeof(data));
        quest(1);
        printf("%d\n",sum);
    }
    return 0;
}
