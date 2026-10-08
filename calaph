#include <ctype.h>
#include <errno.h>
#include <inttypes.h>
#include <limits.h>
#include <math.h>
#include <stdbool.h>
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define INPUT_MAX 4096
#define TEXT_MAX 8192

typedef uint64_t Nat;

typedef struct { const char *s; size_t p; char err[160]; } Parser;

static void err(Parser *p, const char *m) {
    if (!p->err[0]) snprintf(p->err, sizeof p->err, "%s at position %zu", m, p->p);
}
static bool addn(Nat a,Nat b,Nat*r){if(UINT64_MAX-a<b)return false;*r=a+b;return true;}
static bool subn(Nat a,Nat b,Nat*r){if(a<b)return false;*r=a-b;return true;}
static bool muln(Nat a,Nat b,Nat*r){if(a&&b>UINT64_MAX/a)return false;*r=a*b;return true;}
static bool pown(Nat a,Nat b,Nat*r){Nat x=a,y=b,z=1,t;while(y){if(y&1){if(!muln(z,x,&z))return false;}y>>=1;if(y){if(!muln(x,x,&t))return false;x=t;}}*r=z;return true;}

/* Elementary Calaph set. A comma group is the PM; dots left subtract,
   dots right add. A comma group may not be interrupted by dots. */
static bool set_value(Parser*p,Nat*out){
    if(!strncmp(p->s+p->p,"<>",2)){p->p+=2;*out=0;return true;}
    if(p->s[p->p]!='.'&&p->s[p->p]!=','){err(p,"expected Calaph set");return false;}
    size_t left=0,mid=0,right=0; bool comma=false,right_side=false;
    while(p->s[p->p]=='.'||p->s[p->p]==','){
        char c=p->s[p->p++];
        if(c==','){
            if(right_side){err(p,"comma group is interrupted by a dot");return false;}
            comma=true; mid++;
        }else if(comma){right_side=true;right++;}else left++;
    }
    if(!comma){*out=(Nat)left;return true;}
    if(mid>UINT64_MAX/3){err(p,"point-middle overflow");return false;}
    Nat v=(Nat)mid*3,r;
    if(!subn(v,(Nat)left,&r)){err(p,"set is below zero");return false;}
    if(!addn(r,(Nat)right,out)){err(p,"set overflow");return false;}
    return true;
}

static bool expression(Parser*,Nat*);
static bool base(Parser*p,Nat*out){
    if(p->s[p->p]=='('){p->p++;if(!expression(p,out))return false;if(p->s[p->p]!=')'){err(p,"expected )");return false;}p->p++;return true;}
    return set_value(p,out);
}
static bool unary(Parser*p,Nat*out){
    if(p->s[p->p]=='!'&&p->s[p->p+1]=='"'){
        p->p+=2; Nat x; if(!unary(p,&x))return false;
        long double q=sqrtl((long double)x); Nat r=(Nat)q;
        if(q!=(long double)r){err(p,"square root is not natural");return false;}
        *out=r;return true;
    }
    return base(p,out);
}
static bool power(Parser*p,Nat*out){
    Nat a;if(!unary(p,&a))return false;
    if(p->s[p->p]=='\''&&p->s[p->p+1]=='\''){
        p->p+=2;Nat b;if(!power(p,&b))return false;if(!pown(a,b,out)){err(p,"power overflow");return false;}return true;
    }
    *out=a;return true;
}
static bool term(Parser*p,Nat*out){
    Nat v;if(!power(p,&v))return false;
    for(;;){
        if(p->s[p->p]=='\''&&p->s[p->p+1]!='\''){
            p->p++;Nat b;if(!power(p,&b))return false;if(!muln(v,b,&v)){err(p,"multiplication overflow");return false;}
        }else if(p->s[p->p]=='!'&&p->s[p->p+1]=='!'){
            p->p+=2;Nat b;if(!power(p,&b))return false;if(!b){err(p,"division by zero");return false;}if(v%b){err(p,"division is not natural");return false;}v/=b;
        }else break;
    }
    *out=v;return true;
}
static bool expression(Parser*p,Nat*out){
    Nat v;if(!term(p,&v))return false;
    for(;;){
        if(p->s[p->p]==' '){
            if(p->s[p->p+1]==' '){err(p,"multiple spaces are not valid in math expressions");return false;}
            p->p++;Nat b;if(!term(p,&b))return false;if(!addn(v,b,&v)){err(p,"addition overflow");return false;}
        }else if(p->s[p->p]=='!'&&p->s[p->p+1]!='!'&&p->s[p->p+1]!='"'){
            p->p++;Nat b;if(!term(p,&b))return false;if(!subn(v,b,&v)){err(p,"subtraction leaves natural numbers");return false;}
        }else break;
    }
    *out=v;return true;
}
static bool eval(const char*s,Nat*out,char*e,size_t n){Parser p={s,0,{0}};if(!expression(&p,out)||s[p.p]){if(!p.err[0])err(&p,"unexpected character");snprintf(e,n,"%s",p.err);return false;}return true;}

/* A simple non-canonical representation: 1 and 2 use dots; other values
   use a PM of q commas plus r right dots. Other equivalent expressions remain valid. */
static bool encode_nat(Nat n,char*out,size_t cap){
    if(n==0){if(cap<3)return false;strcpy(out,"<>");return true;}
    if(n<3){if(cap<n+1)return false;for(Nat i=0;i<n;i++)out[i]='.';out[n]=0;return true;}
    Nat q=n/3,r=n%3;if(q>(cap-1)||q+r+1>cap)return false;size_t k=0;for(Nat i=0;i<q;i++)out[k++]=',';for(Nat i=0;i<r;i++)out[k++]='.';out[k]=0;return true;
}

static const uint32_t alphabet[] = {
0x41,0x42,0x43,0x44,0x45,0x46,0x47,0x48,0x49,0x4A,0x4B,0x4C,0x4D,0x4E,0xD1,
0x4F,0x50,0x51,0x52,0x53,0x54,0x55,0x56,0x57,0x58,0x59,0x5A,
0x391,0x392,0x393,0x394,0x395,0x396,0x397,0x398,0x399,0x39A,0x39B,0x39C,0x39D,0x39E,0x39F,0x3A0,0x3A1,0x3A3,0x3A4,0x3A5,0x3A6,0x3A7,0x3A8,0x3A9,
0x410,0x411,0x412,0x413,0x414,0x415,0x401,0x416,0x417,0x418,0x419,0x41A,0x41B,0x41C,0x41D,0x41E,0x41F,0x420,0x421,0x422,0x423,0x424,0x425,0x426,0x427,0x428,0x429,0x42A,0x42B,0x42C,0x42D,0x42E,0x42F};
static int alpha_index(uint32_t c){if(c>=0x61&&c<=0x7A)c-=0x20;if(c==0xF1)c=0xD1;for(size_t i=0;i<sizeof(alphabet)/sizeof(alphabet[0]);i++)if(alphabet[i]==c)return(int)i+1;return-1;}
static int utf8(const unsigned char*s,size_t n,size_t*u,uint32_t*c){if(!n)return 0;unsigned char a=s[0];if(a<0x80){*c=a;*u=1;return 1;}if((a&0xE0)==0xC0&&n>=2&&(s[1]&0xC0)==0x80){*c=((a&31)<<6)|(s[1]&63);*u=2;return 1;}if((a&0xF0)==0xE0&&n>=3&&(s[1]&0xC0)==0x80&&(s[2]&0xC0)==0x80){*c=((a&15)<<12)|((s[1]&63)<<6)|(s[2]&63);*u=3;return 1;}if((a&0xF8)==0xF0&&n>=4&&(s[1]&0xC0)==0x80&&(s[2]&0xC0)==0x80&&(s[3]&0xC0)==0x80){*c=((a&7)<<18)|((s[1]&63)<<12)|((s[2]&63)<<6)|(s[3]&63);*u=4;return 1;}return-1;}
static bool put(char*out,size_t cap,size_t*len,const char*s){size_t n=strlen(s);if(n>cap-1-*len)return false;memcpy(out+*len,s,n);*len+=n;out[*len]=0;return true;}
static bool text_encode(const char*in,char*out,size_t cap,char*e,size_t en){size_t i=0,n=strlen(in),l=0;bool first=true;out[0]=0;while(i<n){if(in[i]==' '){if(!put(out,cap,&l,"/")){snprintf(e,en,"output too long");return false;}i++;first=false;continue;}size_t u;uint32_t c;int z=utf8((const unsigned char*)in+i,n-i,&u,&c);if(z<=0){snprintf(e,en,"invalid UTF-8");return false;}int v=alpha_index(c);if(v<0){snprintf(e,en,"unsupported character");return false;}char b[256];encode_nat((Nat)v,b,sizeof b);if(!first&&!put(out,cap,&l,"  ")){snprintf(e,en,"output too long");return false;}if(!put(out,cap,&l,b)){snprintf(e,en,"output too long");return false;}i+=u;first=false;}return true;}
static bool utf8_put(uint32_t c,char*out,size_t cap,size_t*l){char b[5];size_t n=0;if(c<=0x7F)b[n++]=(char)c;else if(c<=0x7FF){b[n++]=(char)(0xC0|(c>>6));b[n++]=(char)(0x80|(c&63));}else{b[n++]=(char)(0xE0|(c>>12));b[n++]=(char)(0x80|((c>>6)&63));b[n++]=(char)(0x80|(c&63));}b[n]=0;return put(out,cap,l,b);}
static bool text_decode(const char*in,char*out,size_t cap,char*e,size_t en){
    size_t i=0,n=strlen(in),l=0;
    out[0]=0;
    while(i<n){
        if(in[i]=='/'){ 
            if(!put(out,cap,&l," ")){snprintf(e,en,"output too long");return false;}
            i++;
        } else {
            size_t st=i;
            while(i<n){
                if(in[i]==' ' && in[i+1]==' ') break;
                i++;
            }
            size_t tl=i-st;
            if(!tl){snprintf(e,en,"invalid encoded character");return false;}
            char tok[512];
            if(tl>=sizeof tok){snprintf(e,en,"encoded expression too long");return false;}
            memcpy(tok,in+st,tl);tok[tl]=0;
            Nat v; char x[160];
            if(!eval(tok,&v,x,sizeof x)||v<1||v>84){
                snprintf(e,en,"invalid encoded character: %.120s",x);return false;
            }
            if(!utf8_put(alphabet[v-1],out,cap,&l)){snprintf(e,en,"output too long");return false;}
        }
        if(i<n && in[i]==' ' && in[i+1]==' '){
            while(i<n && in[i]==' ') i++;
            if(i==n) { snprintf(e,en,"trailing character separator"); return false; }
        }
    }
    return true;
}
static void line(char*b,size_t n){if(!fgets(b,(int)n,stdin)){b[0]=0;return;}size_t l=strlen(b);while(l&& (b[l-1]=='\n'||b[l-1]=='\r'))b[--l]=0;}
static int choice(void){char b[32];line(b,sizeof b);char*e;errno=0;long v=strtol(b,&e,10);if(errno||e==b)return-1;while(isspace((unsigned char)*e))e++;return*e?-1:(int)v;}
static void math_menu(void){for(;;){puts("\nCALAPH 2.0 — MATHEMATICS\n\n1. Evaluate expression\n0. Back\n");printf("> ");int c=choice();if(c==0)return;if(c==1){char b[INPUT_MAX],e[160];Nat v;printf("Expression: ");line(b,sizeof b);if(eval(b,&v,e,sizeof e))printf("= %"PRIu64"\n",v);else printf("Error: %s\n",e);}else puts("Invalid option.");}}
static void text_menu(void){for(;;){puts("\nWORK WITH TEXT\n\n1. Encrypt ordinary text -> Calaph\n2. Decrypt Calaph -> ordinary text\n0. Back\n");printf("> ");int c=choice();if(c==0)return;char b[TEXT_MAX],o[TEXT_MAX],e[160];if(c==1){printf("Text: ");line(b,sizeof b);if(text_encode(b,o,sizeof o,e,sizeof e))puts(o);else printf("Error: %s\n",e);}else if(c==2){printf("Calaph: ");line(b,sizeof b);if(text_decode(b,o,sizeof o,e,sizeof e))puts(o);else printf("Error: %s\n",e);}else puts("Invalid option.");}}
int main(void){for(;;){puts("\nCALAPH 2.0\n\n1. Work with Text\n2. Mathematics\n0. Exit\n");printf("> ");int c=choice();if(c==0)return 0;if(c==1)text_menu();else if(c==2)math_menu();else puts("Invalid option.");}}
