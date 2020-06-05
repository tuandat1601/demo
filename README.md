#include<stdio.h>
#include<string.h>

struct SinhVien;
void hienthi(struct SinhVien sv);
void hienThiDSSV(struct SinhVien* ds, int slsv);
void hienthicot();
struct HoTen{
	char ho[20];
	char dem[21];
	char ten[20];
};
struct ngaythang{
	 int Ngay;   
	 int Thang;    
	 int Nam; 
};
struct SinhVien{
	int ma;
	char que[20];
	struct HoTen hovaten;
	struct ngaythang day;
	char sex[10];
	float diemtb;
	 
	
};
void nhapten(struct HoTen* ten){
printf("Ho: ");
	scanf("%s", ten->ho);
	printf("Dem: ");
	getchar();
	gets(ten->dem);
	printf("Ten: ");
	scanf("%s", ten->ten);

}
void nhapngay(struct ngaythang* day){
	printf("Ngay:");
	scanf("%d",&day->Ngay);
	printf("Thang: ");
   	scanf("%d",&day->Thang);
	printf("Nam: ");
	scanf("%d",&day->Nam);
}


struct SinhVien nhapsv(){
	struct SinhVien sv;
	printf("\nNhap MSSV: ");
	scanf("%d",&sv.ma);
	nhapten(&sv.hovaten);
	nhapngay(&sv.day);
	printf("Gioi tinh: ");
	scanf("%s", sv.sex);
	printf("Que quan: ");
	getchar();
	gets(sv.que);
	printf("Diem trung binh: ");
	scanf("%f",&sv.diemtb);
	return sv;
}
void ghiFile(struct SinhVien* ds,int slsv){
	getchar();
	char fName[26];
	printf("Nhap ten file: ");
	gets(fName);
	FILE* fOut=fopen(fName,"a");
	int i;
	for(i = 0; i < slsv; i++) {
		struct SinhVien sv=ds[i];
		fprintf(fOut,"%-10d %-10s %-20s %-10s %-3d %-3d %-10d %-10s %-15s %-10f\n",
	sv.ma, sv.hovaten.ho, sv.hovaten.dem, sv.hovaten.ten, sv.day.Ngay, sv.day.Thang, sv.day.Nam,
	 sv.sex, sv.que, sv.diemtb);
	fclose(fOut);
	}
}

void docFile(struct SinhVien* ds, int* slsv) {
	FILE* fOut = fopen("demo.txt", "r+");
	int i = 0;
	if(fOut) {
		for(;;) {
			struct SinhVien sv;
		fscanf(fOut,"%10d %10s %20[^\n] %10s %3d %3d %10d %10s %15s %10f\n",
	&sv.ma, sv.hovaten.ho, sv.hovaten.dem, sv.hovaten.ten, &sv.day.Ngay, &sv.day.Thang, &sv.day.Nam,
	 sv.sex, sv.que, &sv.diemtb);
			
			ds[i++] = sv;
			if(feof(fOut)) {
				break;
			}
		}
	}
	
	fclose(fOut);
	*slsv = i;
}
void hienthi(struct SinhVien sv){
	printf("%-10d %-10s %-20s %-10s %-3d %-3d %-15d %-20s %-10s %-10f\n",
	sv.ma, sv.hovaten.ho, sv.hovaten.dem, sv.hovaten.ten, sv.day.Ngay, sv.day.Thang, sv.day.Nam,
	 sv.sex, sv.que, sv.diemtb);
}
void hienthicot(){
	printf("-----------------------------------------------------"
	"----------------------------------------------------------------\n");
printf("%-10s %-10s %-20s %-10s %-3s %-3s %-10s %-20s %-10s %-10s\n", 
	"MSSV", "Ho", "Dem", "Ten", "Ngay","Thang", "Nam", "Gioi Tinh", "Noi o", "Diem TB");
}
void sapxepTheoten(struct SinhVien* ds,int slsv){
	for(int i=0;i<slsv-1;i++){
		for(int j=slsv-1;j>i;j--){
			if(strcmp(ds[j].hovaten.ten,ds[j-1].hovaten.ten)<0){
				struct SinhVien sv=ds[j];
				ds[j]=ds[j-1];
				ds[j-1]= sv;
			}
		}
	}
}
void sapxepTheodiemgiam(struct SinhVien* ds,int slsv){
	for(int i=0;i<slsv-1;i++){
		for(int j=slsv-1;j>i;j--){
			if(ds[j].diemtb>ds[j-1].diemtb){
				struct SinhVien sv=ds[j];
				ds[j]=ds[j-1];
				ds[j-1]= sv;
			}
		}
	}
}
void timten(struct SinhVien* ds,int slsv){
	char ten[20];
	int tim=0;
	printf("Nhap ten can tim : ");
	scanf("%s",ten);
	hienthicot();
	for(int i=0;i<slsv;i++){
		if(strcmp(ten,ds[i].hovaten.ten)==0){
			hienthi(ds[i]);
			tim=1;
		}
	}
	if(tim==0){
		printf("\n Khong co sinh vien trong danh sach!");
	}
}
void timque(struct SinhVien* ds,int slsv){
	char que[20];
	int fin=0;
	printf("Nhap que can tim : ");
	scanf("%s",que);
	for(int i=0;i<slsv;i++){
		if(strcmp(que,ds[i].que)==0){
			hienthi(ds[i]);
			fin=1;
		}
	}
	if(fin==0){
		printf("\n Khong co trong danh sach!",que);
	}
}
void hienThiDSSV(struct SinhVien* ds, int slsv) {
	hienthicot();
	int i;
	for(i = 0; i < slsv; i++) {
		hienthi(ds[i]);
	}
	printf("-----------------------------------------------------"
	"----------------------------------------------------------------\n");
}
int main(){
	SetColor(10);
	struct SinhVien ds[100];
	int slsv=0;
	int luaChon;
	
	docFile(ds, &slsv);
	printf("DANH SACH SINH VIEN HIEN THOI:\n");
	hienThiDSSV(ds, slsv);
	int i;
				
	do {
		printf("=============== MENU ===============");
		printf("\n1. Them Sinh vien vao danh sach.");
		printf("\n2. Hien thi danh sach sinh vien.");
		printf("\n3. Sap xep theo ten.");
		printf("\n4. Sap xep theo diem tb giam dan.");
		printf("\n5. Tim sinh vien theo ten.");
		printf("\n6. Tim sinh vien theo que.");
		printf("\n7. Ghi thong tin sinh vien ra file.");
		printf("\n0. Thoat chuong trinh.");
		printf("\nBan chon ? ");
		
		scanf("%d", &luaChon);
		struct SinhVien sv;
		
		switch(luaChon) {
			case 0:
				break;
				
			case 1:
			struct SinhVien sv;
			sv = nhapsv();
			ds[slsv++]=sv;
				break;
			case 2:
				hienThiDSSV(ds, slsv);
				break;
				
			case 3:
				sapxepTheoten(ds, slsv);
				printf("\nDanh sach sinh vien sau khi sap xep theo ten a-z:\n");
				hienThiDSSV(ds, slsv);
				break;
				
			case 4:
				sapxepTheodiemgiam(ds, slsv);
				printf("\nDanh sach sinh vien sau khi sap xep theo diem giam dan:\n");
				hienThiDSSV(ds, slsv);
				break;
				
			case 5:
				timten(ds, slsv);
				break;
			case 6:timque(ds,slsv);
			break;	
			case 7:
				ghiFile(ds, slsv);
				break;
				
			default:
				printf("Sai chuc nang, vui long chon lai!\n");
				break;
		}
		
	} while(luaChon);
	
	hienthicot();
	
	for(i = 0; i < slsv; i++) {
		hienthi(ds[i]);
	}
	
	return 0;
}

