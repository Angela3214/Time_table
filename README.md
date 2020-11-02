unit Unit1;

{$mode objfpc}{$H+}

interface

uses
  Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, Grids,
  StdCtrls;

type

  { TForm1 }

  TForm1 = class(TForm)
    Button1: TButton;
    Edit1: TEdit;
    StringGrid1: TStringGrid;
    procedure Button1Click(Sender: TObject);
  private

  public

  end;

var
  Form1: TForm1;

implementation

{$R *.lfm}

{ TForm1 }

procedure tabl;
begin
Form1.StringGrid1.FixedCols:=1;//фиксированные колонки
Form1.StringGrid1.FixedRows:=1;//фиксированные строки
Form1.StringGrid1.ColCount:=3;//всего колонок
Form1.StringGrid1.RowCount:=5;//всего строк
Form1.StringGrid1.Cells[0,0]:='Каникулы';//здесь и ниже подписываем колонки и столбцы
Form1.StringGrid1.Cells[1,0]:='Начало';
Form1.StringGrid1.Cells[2,0]:='Конец';
Form1.StringGrid1.Cells[0,1]:='Осенние';
Form1.StringGrid1.Cells[0,2]:='Зимние';
Form1.StringGrid1.Cells[0,3]:='Весенние';
Form1.StringGrid1.Cells[0,4]:='Летние';
end;

procedure ocen(god:integer);//осенние каникулы
var dn,ms:integer; dt:TDateTime;
begin
dn:=01;//берём первое
ms:=11;//ноября
dt:=encodedate(god,ms,dn);//кодируем эту дату
while dayofweek(dt)<>2 do//пока не найдём первый понедельник
  dt:=dt+1;//увеличиваем дату на день
Form1.StringGrid1.Cells[1,1]:=datetostr(dt);//выводим дату начала каникул
dt:=dt+6;//прибавляем шесть дней до воскресенья
Form1.StringGrid1.Cells[2,1]:=datetostr(dt);//выводим дату конца каникул
end;

procedure zim(god:integer);//зимние каникулы
var dt1,dt2:TDateTime; dn1,ms1,dn2,ms2:integer;
begin
dn1:=30;//тридцатое
ms1:=12;//декабря
dt1:=encodedate(god,ms1,dn1);//кодируем дату
Form1.StringGrid1.Cells[1,2]:=datetostr(dt1);//выводим начало каникул
dn2:=03;//третье
ms2:=01;//января
dt2:=encodedate(god+1,ms2,dn2);//кодируем эту дату
while dayofweek(dt2)<>1 do//ищем ближайшее воскресенье
  dt2:=dt2+1;
Form1.StringGrid1.Cells[2,2]:=datetostr(dt2);//выводим дату конца каникул
end;

procedure vesna(god:integer);
var dt:TDateTime; dn,ms:integer;
begin
dn:=25;//двадцать пятое
ms:=03;//марта (берём эту дату, так как она отстаёт на неделю от конца месяца,
dt:=encodedate(god+1,ms,dn);                        //и должен попасться понедельник)
while dayofweek(dt)<>2 do//пока не надём понедельник
  dt:=dt+1;//увеличиваем дату на единицу
Form1.StringGrid1.Cells[1,3]:=datetostr(dt);//выводим дату начала каникул
dt:=dt+6;//прибавляем шесть дней до воскресенья
Form1.StringGrid1.Cells[2,3]:=datetostr(dt);//выводим дату конца каникул
end;

procedure leto(god:integer);
var dt1,dt2:TDateTime; dn1,ms1,dn2,ms2:integer;
begin
dn1:=25;//двадцать пятое
ms1:=06;//июня
dt1:=encodedate(god+1,ms1,dn1);//кодируем дату
Form1.StringGrid1.Cells[1,4]:=datetostr(dt1);//выводим дату начала каникул
dn2:=31;//тридцать первое
ms2:=08;//июня
dt2:=encodedate(god+1,ms2,dn2);//кодируем дату
while dayofweek(dt2)<>1 do//пока не найдём ближайшее воскресенье
  dt2:=dt2+1;//увеличиваем дату на день
Form1.StringGrid1.Cells[2,4]:=datetostr(dt2);//выводим дату конца каникул
end;

procedure TForm1.Button1Click(Sender: TObject);
var god:integer;
begin
tabl;//создаём таблицу
god:=strtoint(Form1.Edit1.Text);//берём номер года из поля ввода
ocen(god);//осенние каникулы
zim(god);//зимние каникулы
vesna(god);//весенние каникулы
leto(god);//летние каникулы
end;

end.
