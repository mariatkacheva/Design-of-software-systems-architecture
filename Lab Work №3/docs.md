# Лабораторная работа №3

> Тема:  Использование принципов проектирования на уровне методов и классов.
> 
> Цель работы: Получить опыт проектирования и реализации модулей с использованием принципов KISS, YAGNI, DRY, SOLID и др.
> 
> Ожидаемые результаты:
> 
> Для выбранного варианта использования:
> 
> 1. Добавить ранее созданные или создать диаграммы контейнеров и компонентов нотации C4 model. (1 балл)
> 2. Построить диаграмму последовательностей для выбранного варианта использования (показать взаимодействие C4-компонентов для реализации выбранного варианта использования). (2 балла)
> 3. Построить модель БД в виде диаграммы классов UML. Если по заданию не предусмотрена БД, то самостоятельно продумать возможное хранилище данных, связанное с заданием. Минимально количество сущностей: 5. (1 балл)
> 4. Реализовать требуемый клиентский и серверный код с учетом принципов KISS, YAGNI, DRY и SOLID. Пояснить, каким образом были учтены эти принципы. (4  балла)

# Результаты:
**Диаграмма вариантов использования:**

Для выполнения работы был выбран вариант использования - "Согласовать документ".

![Диаграмма вариантов использования drawio (1)](https://github.com/user-attachments/assets/d63e0c6e-1c26-4535-817b-ca284dffe55d)

## 1.  Диаграммы контейнеров и компонентов нотации C4 model. 

**Диаграмма контейнеров для Информационной Системы Управления Землями:**

![Диаграмма контейнеров drawio (1)](https://github.com/user-attachments/assets/85b9f373-7445-419f-ae4b-03117b65c754)

**Диаграммы компонентов:**

### Контейнер 1: Подсистема согласования документов

![Диаграмма компонентов 1 drawio (1)](https://github.com/user-attachments/assets/18c922a9-9b5e-4558-b43e-446f9edca042)

### Контейнер 2: Веб-клиент

![Диаграмма компонентов 2 drawio](https://github.com/user-attachments/assets/793729ba-2864-4d71-bf35-8c310ab2369a)

## 2.  Диаграмма последовательностей для варианта использования "Согласовать документ".
Для наглядности процесса согласования документа в диаграмму был добавлен объект "База данных ИСУЗ" являющаяся не компонентом, а контейнером.

![Диаграмма последовательностей _Согласовать документ_ drawio (2)](https://github.com/user-attachments/assets/0fb22b62-8e2e-4724-8752-fca076643fad)

## 3.  Модель базы данных для варианта использования "Согласовать документ".
**Диаграмма классов для процесса согласования документа**

![image](https://github.com/user-attachments/assets/800ce2ae-9c81-4981-9cad-5f93a72b92f6)

## 4.  Клиентский и серверный код с учетом принципов KISS, YAGNI, DRY и SOLID для варианта использования "Согласовать документ". 
Описанный ниже код представлен в сокращенном виде по двум причинам: 
1. Для того, чтобы не нарушать рабочее соглашение о неразлашении
2. Для того, чтобы представить основную концепцию модуля, не перегружая этот отчет, так как в представленном программном коде уже поддерживается не только логика для согласования, но и логика подписания, возврата на доработку и т.д.

**Клиентский код**

```aspx
<%@ Page Language="C#" MasterPageFile="~/Site1.Master" AutoEventWireup="true" CodeBehind="SoglasovanieDokumentaWebE.aspx.cs" Inherits="IIS.ISUZ.Web.СогласованиеДокументаWebE" EnableEventValidation="false" %>

<%@ Import Namespace="IIS.ISUZ" %>
<%@ Import Namespace="IIS.ISUZ.Tools" %>

<%@ Register Src="~/ICSSoft.STORMNET.Web.Controls/DatePicker.ascx" TagName="DatePicker" TagPrefix="uc3" %>
<%@ Register Src="~/Controls/ViewAudit.ascx" TagName="ViewAudit" TagPrefix="au" %>
<%@ Register Src="~/Controls/FileUpload.ascx" TagName="FileUpload" TagPrefix="fu" %>

<asp:Content ID="PageTitleContent" ContentPlaceHolderID="PageTitlePlaceholder" runat="server">
    ИСУЗ - Согласование документа
</asp:Content>
<asp:Content ID="H1Content" ContentPlaceHolderID="H1ContentPlaceHolder" runat="server">
    Согласование документа
</asp:Content>

<asp:Content ID="Buttons" ContentPlaceHolderID="ButtonsPlaceHolder" runat="server">
    <div id="actionButtons">

        <asp:ImageButton ID="SaveBtn" runat="server" SkinID="SaveBtn" OnClick="SaveBtn_Click" AlternateText="<%$ Resources: Resource, Save %>" ValidationGroup="savedoc" ToolTip="Сохранить"/>
        <asp:ImageButton ID="SaveAndCloseBtn" runat="server" SkinID="SaveAndCloseBtn" OnClick="SaveAndCloseBtn_Click" AlternateText="<%$ Resources: Resource, Save_Close %>" ValidationGroup="savedoc"  ToolTip="Сохранить и закрыть"/>
        <asp:ImageButton ID="CancelBtn" runat="server" SkinID="CancelBtn" OnClick="CancelBtn_Click" AlternateText="<%$ Resources: Resource, Cancel %>" ToolTip="Отмена"/>
        <asp:ImageButton ID="ImageButtonPrint" runat="server" SkinID="ApprovalSheet" OnClick="ImageButtonPrint_Click" AlternateText="Лист согласований" ToolTip="Лист согласований" />

        <asp:Button ID="ctrlОтправитьНаСогласованиеПодписание" runat="server" Text="Отправить на согласование/подписание" OnClientClick="return CallDialogOtprSog();"  OnClick="ctrlОтправитьНаСогласованиеПодписание_Click" Style="height: 25px; vertical-align:top;" />
        <asp:Button ID="ctrlСогласоватьВернутьИсполнителю" runat="server" Text="Согласовать/вернуть исполнителю" OnClientClick="return CallDialogSog();" OnClick="ctrlСогласоватьВернутьИсполнителю_Click" Style="height: 25px; vertical-align:top;" />
        <asp:Button ID="ctrlПодписать" runat="server" Text="Подписать" OnClientClick="OnSignBtn(false); return false;" OnClick="CtrlПодписать_Click" Style="height: 25px; vertical-align:top;" />
        <asp:Button runat="server" ID="ctrlСформироватьФайл" Text="Сформировать файл" OnClientClick="OnCreateVisualFileBtn(); return false;" Style="height: 25px; vertical-align:top;" />
        <asp:Button runat="server" ID="ctrlСформироватьЛистСогласований" Text="Сформировать лист согласований" OnClientClick="ShowSTPDialog('ЛистСогласований'); return false;" Style="height: 25px; vertical-align:top;" />
    </div>
</asp:Content>

<asp:Content ID="Content1" ContentPlaceHolderID="ContentPlaceHolder1" runat="server">
    <div id="mainContent">
        <br />
        <asp:ScriptManager ID="ScriptManager1" EnablePageMethods="True" runat="server">
        </asp:ScriptManager>

        <asp:ValidationSummary ID="ValidationSummary1" runat="server" DisplayMode="List"
            EnableClientScript="true" ValidationGroup="savedoc" CssClass="ValidationSummary1"
            ShowMessageBox="false" ShowSummary="true" HeaderText="" />

        <asp:CustomValidator ID="SoglOrPodpisantValidator" runat="server" EnableClientScript="true"
            Display="None" ErrorMessage="Необходимо заполнить хотя бы одного подписанта. А так же удалить строку или выбрать сотрудника если есть пустые строки в таблицах Согласующий или Подписант"
            ClientValidationFunction="SoglOrPodpisantValidate"
            ValidationGroup="savedoc"></asp:CustomValidator>

        <asp:CustomValidator ID="SogAndPodpisantOdinakovValidator" runat="server" EnableClientScript="true"
            Display="None" ErrorMessage="Согласующий и подписант не может быть одним лицом"
            ClientValidationFunction="SogAndPodpisantOdinakovValidate"
            ValidationGroup="savedoc"></asp:CustomValidator>

        <div id="divДокумент" runat="server" class="clearfix">
            <asp:Label CssClass="descLabel percent-12" ID="ctrlДокументLabel" runat="server" Text="Документ" EnableViewState="False">
            </asp:Label>
            <ac:MasterEditorAjaxLookUp ID="ctrlДокумент" CssClass="descText percent-59 ui-autocomplete-input" runat="server" Enabled="false" ShowInThickBox="True" Autocomplete="true" IsSubstring="True"/>
        </div>
        <div id="divСостояние" class="clearfix">
            <asp:Label CssClass="descLabel percent-12" ID="ctrlСостояниеLabel" runat="server" Text="Состояние" EnableViewState="False">
            </asp:Label>
            <asp:DropDownList ID="ctrlСостояние" CssClass="descText percent-59" runat="server" Enabled="false">
                <asp:ListItem>Не начато</asp:ListItem>
                <asp:ListItem>В процессе</asp:ListItem>
                <asp:ListItem>На подписании</asp:ListItem>
                <asp:ListItem>На регистрации</asp:ListItem>
                <asp:ListItem>Завершено</asp:ListItem>
                <asp:ListItem>Отменено</asp:ListItem>
                <asp:ListItem>На доработке</asp:ListItem>
            </asp:DropDownList>
        </div>
        <div class="clearfix" id="divПлановыйСрокИсполненияЗаявления" runat="server">
            <asp:Label CssClass="descLabel percent-12" ID="ctrlПлановыйСрокИсполненияЗаявленияLabel" runat="server" Text="Плановый срок исполнения заявления" EnableViewState="False" />
            <uc3:DatePicker ID="ctrlПлановыйСрокИсполненияЗаявления" runat="server" Enabled="false"></uc3:DatePicker>
        </div>

        <div id="divОсновнойИсполнитель" class="clearfix">
            <asp:Label CssClass="descLabel percent-12" ID="ctrlОсновнойИсполнительLabel" runat="server" Text="Основной исполнитель" EnableViewState="False" />
            <ac:MasterEditorAjaxLookUp ID="ctrlОсновнойИсполнитель" CssClass="descText percent-59 ui-autocomplete-input" runat="server" ShowInThickBox="True" Autocomplete="true" IsSubstring="True" Enabled="false"/>           
        </div>

        <div id="divПодготовкаСхемыЗУ" class="clearfix">
            <asp:Label CssClass="descLabel percent-12" ID="ctrlПодготовкаСхемыЗУLabel" runat="server" Text="Подготовка схемы ЗУ" EnableViewState="False" />
            <asp:CheckBox ID="ctrlПодготовкаСхемыЗУ" CssClass="descCheckBox percent-2" runat="server" Text="" />
        </div>

        <div id="divРегистратор" class="clearfix">
            <asp:Label CssClass="descLabel percent-12" ID="ctrlРегистраторLabel" runat="server" Text="Регистратор" EnableViewState="False" />
            <ac:MasterEditorAjaxLookUp ID="ctrlРегистратор" CssClass="descText percent-59 ui-autocomplete-input" runat="server" ShowInThickBox="True" Autocomplete="true" IsSubstring="True" />
        </div>
        <br />

        <fieldset>
            <legend>
                <span>Согласующие</span>
            </legend>
            <div id="divСогласующий" class="clearfix">
                <ac:AjaxGroupEdit runat="server" ID="ctrlСогласующий" ReadOnly="False"/>
            </div>        
        </fieldset>

        <fieldset>
            <legend>
                <span>Подписанты</span>
            </legend>
            <div id="divПодписант" class="clearfix">
                <ac:AjaxGroupEdit runat="server" ID="ctrlПодписант" ReadOnly="False"/>
            </div>        
        </fieldset>

        <fieldset>
            <legend>
                <span>Файлы проекта документа</span>
            </legend>
            <fu:FileUpload ID="ctrПоследнийЗагруженныйФайлUpload" runat="server" Header="Последний загруженный файл"/>
            <fu:FileUpload ID="ctrФайлСоСхемойЗУUpload" runat="server" Header="Файл со схемой ЗУ"/>
            <fu:FileUpload ID="ctrДополнительныйФайлUpload" runat="server" Header="Дополнительный файл к согласованию"/>
        </fieldset>

        <fu:FileUpload ID="ctrФайлДляПечатиUpload" runat="server" Header="Файл для печати"/>

        <fu:FileUpload ID="ctrФайлПодписиUpload" runat="server" Header="Файл подписи"/>

        <fu:FileUpload ID="ctrlПоследнийЛистСогласованийUpload" runat="server" Header="Лист согласований" />

        <fu:FileUpload ID="ctrlЛистСогласованийДляПечатиUpload" runat="server" Header="Лист согласований для печати" />

        <fieldset>
            <legend>
                <span>Архив исполнителей</span>
            </legend>
            <div id="divИсполнитель" class="clearfix">
                <asp:Label CssClass="descLabel percent-12" ID="ctrИсполнительLabel" runat="server" Text="Текущий исполнитель/согласующий" EnableViewState="False">
                </asp:Label>
                <ac:MasterEditorAjaxLookUp ID="ctrlТекущийИсполнитель" CssClass="descText percent-59 ui-autocomplete-input" runat="server" ShowInThickBox="True" Autocomplete="true" IsSubstring="True" Enabled="false"/>
            </div>
            <ac:WebObjectListView ID="ctrlArchSoglasovaniya" runat="server" readonly="false"/>
        </fieldset>
    </div>
    <input type="hidden" id="hidSoglas" runat="server" />
    <input type="hidden" id="hidPodis" runat="server" />

    <div id="dialog-otpr-sog" title="Согласование" style="display:none">
        <div class="clearfix" style="margin-top: 10px;">
            <span class="descLabel" style="text-align:left;">Комментарий</span>
            <textarea id="commentOtpSog" runat="server" rows="4" cols="20" class="descText percent-jp-90"></textarea>
        </div>
        <div class="clearfix">
            <fu:FileUpload ID="ctrlМодалОтпрСогФайлПроектаUpload" runat="server" Header="Файл проекта"/>
            <asp:Label CssClass="descLabel percent-90" style="color: red;" ID="ctrlМодалОтпрСогФайлПроектаUploadErrorLabel" runat="server" Text="Необходимо заполнить 'Файл проекта'" EnableViewState="False">
            </asp:Label>
            <asp:Label CssClass="descLabel percent-90" style="color: red;" ID="ctrlМодалОтпрСогФайлПроектаUploadErrorExtenLabel" runat="server" Text="Загруженный файл должен быть формата pdf или docx/doc" EnableViewState="False">
            </asp:Label>
        </div>
        <div class="clearfix" id="divМодалОтпрСогФайлСхемаЗУUpload" runat="server">
            <fu:FileUpload ID="ctrlМодалОтпрСогФайлСхемаЗУUpload" runat="server" Header="Файл со схемой ЗУ"/>
            <asp:Label CssClass="descLabel percent-90" style="color: red;" ID="ctrlМодалОтпрСогФайлСхемаЗУUploadErrorLabel" runat="server" Text="Необходимо заполнить 'Файл со схемой ЗУ'" EnableViewState="False">
            </asp:Label>
            <asp:Label CssClass="descLabel percent-90" style="color: red;" ID="ctrlМодалОтпрСогФайлСхемаЗУUploadErrorExtenLabel" runat="server" Text="Загруженный файл должен быть формата pdf или docx/doc" EnableViewState="False">
            </asp:Label>
        </div>
        <div class="clearfix">
            <fu:FileUpload ID="ctrlМодалОтпрСогФайлДопUpload" runat="server" Header="Дополнительный файл к согласованию"/>
        </div>
    </div>

    <div id="dialog-sog" title="Согласование" style="display:none">
        <div class="clearfix" style="display: flex;flex-wrap: wrap;">
            <div style="width:50%;">
                <span class="descLabel percent-12" style="text-align:left;">Согласовано</span>
                <asp:DropDownList ID="ctrlSoglasovano" CssClass="descText percent-jp-80" runat="server">
                    <asp:ListItem>Нет</asp:ListItem>
                    <asp:ListItem>Да</asp:ListItem>
                </asp:DropDownList>
            </div>
            <div style="width:50%;">
                <span class="descLabel percent-12" style="text-align:left;">Дата исполнения</span>
                <uc3:DatePicker ID="execdate" CssClass="descText percent-jp-80" runat="server" Enabled="false"/>
            </div>
        </div>
        
        <div class="clearfix" style="margin-top: 10px;">
            <span class="descLabel" style="text-align:left;">Комментарий</span>
            <textarea id="commentSog" rows="4" cols="20" runat="server" class="descText percent-jp-90"></textarea>
        </div>

        <div class="clearfix">
            <fu:FileUpload ID="ctrlМодалСогФайлПроектаUpload" runat="server" Header="Файл проекта"/>
            <asp:Label CssClass="descLabel percent-90" style="color: red;" ID="ctrlМодалСогФайлПроектаUploadErrorExtenLabel" runat="server" Text="Загруженный файл должен быть формата pdf или docx/doc" EnableViewState="False">
            </asp:Label>
        </div>
        <div class="clearfix" id="divМодалСогФайлСхемаЗУUpload" runat="server">
            <fu:FileUpload ID="ctrlМодалСогФайлСхемаЗУUpload" runat="server" Header="Файл со схемой ЗУ"/>
            <asp:Label CssClass="descLabel percent-90" style="color: red;" ID="ctrlМодалСогФайлСхемаЗУUploadErrorExtenLabel" runat="server" Text="Загруженный файл должен быть формата pdf или docx/doc" EnableViewState="False">
            </asp:Label>
        </div>
        <div class="clearfix">
            <fu:FileUpload ID="ctrlМодалСогФайлДопUpload" runat="server" Header="Дополнительный файл к согласованию"/>
        </div>
    </div>

    <div id="stamps-to-file-dialog-form">
        <div class="row">
            <div class="span12" id="pdfManager" style="display: none;">
                <div class="row" id="selectorContainer" style="display:inline-flex">
                    <div class="span3">
                        <div class="col-fixed-240" id="stampsContainer">
                            <div class="stamps-layer" style="height:0px;"></div>
                            <div class="text-layer"></div>
                        </div>
                    </div>
                    <div class="span9">
                        <div class="col-fixed-605">
                            <div id="pageContainer" class="pdfViewer singlePageView dropzone nopadding portrait" style="background-color: transparent">
                                <asp:Button OnClientClick="changePage(-1); return false;" runat="server" ID="previousPage" Text="<" CssClass="page-arrows"></asp:Button>
                                <canvas id="the-canvas" style="border: 1px  solid black"></canvas>
                                <asp:Button OnClientClick="changePage(1); return false;" runat="server" ID="nextPage" Text=">" CssClass="page-arrows nextPage"></asp:Button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

</asp:Content>
```

**Серверный код**
```csharp
// --------------------------------------------------------------------------------------------------------------------
// <copyright file="SoglasovanieDokumentaWebE.aspx.cs" company="IIS">
//   Copyright (c) IIS. All rights reserved.
// </copyright>
// <summary>
//   Defines the СогласованиеДокументаWebE type.
// </summary>
// --------------------------------------------------------------------------------------------------------------------

namespace IIS.ISUZ.Web
{
    using ICSSoft.STORMNET;
    using ICSSoft.STORMNET.Business;
    using ICSSoft.STORMNET.FunctionalLanguage;
    using ICSSoft.STORMNET.FunctionalLanguage.SQLWhere;
    using ICSSoft.STORMNET.Web;
    using ICSSoft.STORMNET.Web.Tools;
    using ICSSoft.STORMNET.Windows.Forms;
    using IIS.ISUZ.Helpers;
    using IIS.ISUZ.ReportService;
    using IIS.ISUZ.SignService;
    using IIS.ISUZ.Tools;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Security.Cryptography.X509Certificates;
    using System.Web.Services;

    public partial class СогласованиеДокументаWebE : BaseEditForm<СогласованиеДокумента>
    {
        private readonly ИСУЗBS businessServer = new ИСУЗBS();

        public string parameters = "[]";
        public bool isIframe => Request["inframe"] == "1";

        /// <summary>
        /// "~/forms/SoglasovanieDokumenta/SoglasovanieDokumentaWebE.aspx"
        /// </summary>
        public static string ServerFormPath
        {
            get
            {
                return "~/" + ClientFormPath;
            }
        }

        /// <summary>
        /// "forms/SoglasovanieDokumenta/SoglasovanieDokumentaWebE.aspx"
        /// </summary>
        public static string ClientFormPath
        {
            get
            {
                return "forms/SoglasovanieDokumenta/SoglasovanieDokumentaWebE.aspx";
            }
        }

        /// <summary>
        /// Конструктор формы
        /// </summary>
        public СогласованиеДокументаWebE()
            : base(СогласованиеДокумента.Views.СогласованиеДокументаWebE)
        {

        }

        protected override void PreApplyToControls()
        {
            base.PreApplyToControls();

            ctrlПодготовкаСхемыЗУ.Enabled = DataObject.Состояние == СостояниеСогласования.НеНачато || IsObjectCreated;

            divДокумент.Visible = !isIframe;

            ctrlРегистратор.Enabled = DataObject.Состояние == СостояниеСогласования.НеНачато || DataObject.Состояние == СостояниеСогласования.ВПроцессе
                || DataObject.Состояние == СостояниеСогласования.НаДоработке;

            ctrlОтправитьНаСогласованиеПодписание.Visible = (DataObject.Состояние == СостояниеСогласования.НеНачато || DataObject.Состояние == СостояниеСогласования.НаДоработке
                || (DataObject.Состояние == СостояниеСогласования.ВПроцессе && DataObject.ОтправленоНаСогласование == false)
                || (DataObject.Состояние == СостояниеСогласования.НаПодписании && DataObject.ОтправленоНаСогласование == false)) && Tools.Constants.EQPK(DataObject.ТекущийИсполнитель?.__PrimaryKey, Сотрудник.Текущий?.__PrimaryKey);

            ctrlПодписать.Visible = DataObject.АрхивСогласования.Cast<АрхивСогласования>().OrderByDescending(x => x.CreateTime).FirstOrDefault(x =>
                x.Подписант && x.Согласовано == Согласовано.Да && !x.Подписано && Tools.Constants.EQPK(x.Исполнитель?.__PrimaryKey, Сотрудник.Текущий.__PrimaryKey)) != null;

            ctrlСогласоватьВернутьИсполнителю.Visible = !ctrlПодписать.Visible
                && (DataObject.Состояние == СостояниеСогласования.ВПроцессе || DataObject.Состояние == СостояниеСогласования.НаПодписании) && DataObject.ОтправленоНаСогласование == true
                && Tools.Constants.EQPK(DataObject.ТекущийИсполнитель?.__PrimaryKey, Сотрудник.Текущий?.__PrimaryKey);

            ctrlСформироватьФайл.Visible = (DataObject.Состояние == СостояниеСогласования.НаРегистрации || DataObject.Состояние == СостояниеСогласования.Завершено)
                && Сотрудник.Текущий != null && Сотрудник.Текущий.РегистраторСогласования
                && DataObject.ФайлДляШтампов != null && DataObject.ФайлПодписи != null;

            var access = new OperationAccess();

            ctrlОтправитьНаСогласованиеПодписание.Enabled = access.СогласованиеОтправкаНаСогласование || access.СогласованиеИсполнитель;
            ctrlПодписать.Enabled = access.СогласованиеПодписаниеЭЦП;
            ctrlСогласоватьВернутьИсполнителю.Enabled = access.СогласованиеСогласоватьВернутьИсполнителю;
            ctrlСформироватьФайл.Enabled = access.СогласованиеРегистратор;

            ctrlСогласующий.Enabled = access.СогласованиеИсполнитель;
            ctrlПодписант.Enabled = access.СогласованиеИсполнитель;
            ctrlРегистратор.Enabled = access.СогласованиеИсполнитель;

            // Чтобы кнопки выбора также становились неактивными
            ctrlСогласующий.ReadOnly = !access.СогласованиеИсполнитель;
            ctrlПодписант.ReadOnly = !access.СогласованиеИсполнитель;
           
            ctrlСогласующий.ColSortDef = new ColumnsSortDef[]
                {
                    new ColumnsSortDef(Information.ExtractPropertyName<Согласующий>(x => x.ПорядковыйНомерСогласующего), SortOrder.Asc)
                };

            ctrlПодписант.ColSortDef = new ColumnsSortDef[]
                {
                    new ColumnsSortDef(Information.ExtractPropertyName<Подписант>(x => x.ПорядковыйНомерПодписанта), SortOrder.Asc)
                };

            ctrlПодписант.Operations.UserColumnSort = false;
            ctrlСогласующий.Operations.UserColumnSort = false;
            ctrlПодписант.Operations.AllowColumnResizing = false;
            ctrlСогласующий.Operations.AllowColumnResizing = false;
            ctrlПодписант.Operations.Delete = false;
            ctrlСогласующий.Operations.Delete = false;

            ctrlПодписант.Operations.Add = DataObject.Состояние != СостояниеСогласования.НаПодписании;

            hidPodis.Value = "no";
            hidSoglas.Value = "no";

            if (DataObject.Состояние != СостояниеСогласования.НеНачато)
            {
                if (DataObject.Согласующий.Cast<Согласующий>().Any(x => Tools.Constants
                    .EQPK(DataObject.ТекущийИсполнитель?.__PrimaryKey, x.СогласующийСотрудник?.__PrimaryKey)))
                {
                    hidSoglas.Value = DataObject.Согласующий.Cast<Согласующий>().Where(x => Tools.Constants
                    .EQPK(DataObject.ТекущийИсполнитель?.__PrimaryKey, x.СогласующийСотрудник?.__PrimaryKey)).FirstOrDefault()?.СогласующийСотрудник?.__PrimaryKey?.ToString();
                }

                if (DataObject.Подписант.Cast<Подписант>().Any(x => Tools.Constants
                    .EQPK(DataObject.ТекущийИсполнитель?.__PrimaryKey, x.ПодписантСотрудник?.__PrimaryKey)))
                {
                    hidSoglas.Value = "all";
                    ctrlСогласующий.Operations.Add = false;
                    hidPodis.Value = DataObject.Подписант.Cast<Подписант>().Where(x => Tools.Constants
                    .EQPK(DataObject.ТекущийИсполнитель?.__PrimaryKey, x.ПодписантСотрудник?.__PrimaryKey)).FirstOrDefault()?.ПодписантСотрудник?.__PrimaryKey?.ToString();
                }
            }

            if (DataObject.Состояние == СостояниеСогласования.Завершено || DataObject.Состояние == СостояниеСогласования.Отменено 
                || DataObject.Состояние == СостояниеСогласования.НаРегистрации)
            {
                hidPodis.Value = "all";
                hidSoglas.Value = "all";
                ctrlСогласующий.Operations.Add = false;
                ctrlПодписант.Operations.Add = false;
            }


            if (!Tools.Constants.EQPK(DataObject.ОсновнойИсполнитель?.__PrimaryKey, Сотрудник.Текущий.__PrimaryKey))
            {
                SaveAndCloseBtn.Visible = false;
                SaveBtn.Visible = false;
                ctrlРегистратор.Enabled = false;
                hidPodis.Value = "all";
                hidSoglas.Value = "all";
                ctrlСогласующий.Operations.Add = false;
                ctrlПодписант.Operations.Add = false;
            }

            ImageButtonPrint.Visible = DataObject.Состояние == СостояниеСогласования.Завершено;
            ctrlСформироватьЛистСогласований.Visible = DataObject.ПоследнийЛистСогласований != null;

            НастройкаЛукапов();
        }

        public override void Refresh(Dictionary<string, string> param = null)
        {
            if (isIframe)
            {
                PageContentManager.AttachJavaScriptCode(@"
                    ShowWaitMessage('Сохранение изменений...');
                    window.top.location.reload(true);
                "); //если согласование используется в iframe, перезагрузить iframe вместе с родительской (основной) страницей
            }
            else
            {
                base.Refresh(param);
            }
        }

        /// <summary>
        /// Здесь лучше всего изменять свойства контролов на странице, которые не обрабатываются WebBinder
        /// </summary>
        protected override void PostApplyToControls()
        {
            base.PostApplyToControls();

            НастройкаWOLV();

            ctrПоследнийЗагруженныйФайлUpload.DataObject = DataObject.ПоследнийЗагруженныйФайл;
            ctrФайлСоСхемойЗУUpload.DataObject = DataObject.ФайлСоСхемойЗУ;
            ctrДополнительныйФайлUpload.DataObject = DataObject.ДополнительныйФайл;
            ctrФайлДляПечатиUpload.DataObject = DataObject.ФайлДляПечати;
            ctrФайлПодписиUpload.DataObject = DataObject.ФайлПодписи;
            ctrlПоследнийЛистСогласованийUpload.DataObject = DataObject.ПоследнийЛистСогласований;
            ctrlЛистСогласованийДляПечатиUpload.DataObject = DataObject.ЛистСогласованийДляПечати;

            var parameters = GetPlaceholderParameters(DataObject);
            this.parameters = JsonConvert.SerializeObject(parameters);
        }

        private void НастройкаЛукапов()
        {
            ctrПоследнийЗагруженныйФайлUpload.ObjType = typeof(СогласованиеДокумента).AssemblyQualifiedName;
            ctrФайлСоСхемойЗУUpload.ObjType = typeof(СогласованиеДокумента).AssemblyQualifiedName;
            ctrДополнительныйФайлUpload.ObjType = typeof(СогласованиеДокумента).AssemblyQualifiedName;
            ctrФайлДляПечатиUpload.ObjType = typeof(СогласованиеДокумента).AssemblyQualifiedName;
            ctrФайлПодписиUpload.ObjType = typeof(СогласованиеДокумента).AssemblyQualifiedName;
            ctrlПоследнийЛистСогласованийUpload.ObjType = typeof(СогласованиеДокумента).AssemblyQualifiedName;
            ctrlЛистСогласованийДляПечатиUpload.ObjType = typeof(СогласованиеДокумента).AssemblyQualifiedName;

            ctrlМодалОтпрСогФайлПроектаUpload.ObjType = typeof(СогласованиеДокумента).AssemblyQualifiedName;
            ctrlМодалОтпрСогФайлСхемаЗУUpload.ObjType = typeof(СогласованиеДокумента).AssemblyQualifiedName;
            ctrlМодалОтпрСогФайлДопUpload.ObjType = typeof(СогласованиеДокумента).AssemblyQualifiedName;

            ctrlМодалСогФайлПроектаUpload.ObjType = typeof(СогласованиеДокумента).AssemblyQualifiedName;
            ctrlМодалСогФайлСхемаЗУUpload.ObjType = typeof(СогласованиеДокумента).AssemblyQualifiedName;
            ctrlМодалСогФайлДопUpload.ObjType = typeof(СогласованиеДокумента).AssemblyQualifiedName;

            ctrПоследнийЗагруженныйФайлUpload.ReadOnly = ctrФайлСоСхемойЗУUpload.ReadOnly =
            ctrДополнительныйФайлUpload.ReadOnly = ctrФайлДляПечатиUpload.ReadOnly = ctrФайлПодписиUpload.ReadOnly =
            ctrlПоследнийЛистСогласованийUpload.ReadOnly = ctrlЛистСогласованийДляПечатиUpload.ReadOnly = true;

            ctrlРегистратор.LimitFunction = FunctionBuilder.BuildEquals<Сотрудник>(x => x.РегистраторСогласования, true);

            if (DataObject.Документ != null && !IsPostBack)
            {
                if (DataObject.Документ.ТипДокумента.Equals("Распоряжение"))
                {
                    ctrlДокумент.ShowObjectUrl = ДокументРаспоряжениеWebE.ClientFormPath;
                }
                else
                {
                    ctrlДокумент.ShowObjectUrl = ДокументWebE.ClientFormPath + "?tip=out";
                }
            }
        }

        protected override bool PreSaveObject()
        {
            if (DataObject.Согласующий.Count == 0 && DataObject.Подписант.Count == 0)
            {
                PageContentManager.AttachJavaScriptCode($"jAlert('Не указаны согласующие или подписанты', 'Сохранение невозможно');", true);
                return false;
            }

            if (DataObject.Согласующий.Cast<Согласующий>().Any(x => x.GetStatus() == ObjectStatus.Deleted))
            {
                var deletedSogs = DataObject.Согласующий.Cast<Согласующий>().Where(x => x.GetStatus() == ObjectStatus.Deleted).OrderBy(x => x.ПорядковыйНомерСогласующего).ToList();

                foreach(var deletedSog in deletedSogs)
                {
                    foreach(var sog in DataObject.Согласующий.Cast<Согласующий>())
                    {
                        if (sog.ПорядковыйНомерСогласующего > deletedSog.ПорядковыйНомерСогласующего)
                            sog.ПорядковыйНомерСогласующего--;
                    }
                }
            }

            if (DataObject.Подписант.Cast<Подписант>().Any(x => x.GetStatus() == ObjectStatus.Deleted))
            {
                var deletedPodps = DataObject.Подписант.Cast<Подписант>().Where(x => x.GetStatus() == ObjectStatus.Deleted).OrderBy(x => x.ПорядковыйНомерПодписанта).ToList();

                foreach (var deletedPodp in deletedPodps)
                {
                    foreach (var sog in DataObject.Подписант.Cast<Подписант>())
                    {
                        if (sog.ПорядковыйНомерПодписанта > deletedPodp.ПорядковыйНомерПодписанта)
                            sog.ПорядковыйНомерПодписанта--;
                    }
                }
            }

            return base.PreSaveObject();
        }

        private void НастройкаWOLV()
        {
            var wsa = new WOLVSettApplyer();

            ctrlArchSoglasovaniya.View = АрхивСогласования.Views.АрхивСогласованияWebD;
            ctrlArchSoglasovaniya.LimitFunction =
                businessServer.GetLFForDetailByMaster(
                    Information.ExtractPropertyName<АрхивСогласования>(x => x.Согласование),
                    DataObject.__PrimaryKey.ToString());

            wsa.SettingsApply(ctrlArchSoglasovaniya);

            ctrlArchSoglasovaniya.Operations.EditOnClickInRow = false;
            ctrlArchSoglasovaniya.Operations.New = false;
            ctrlArchSoglasovaniya.Operations.Delete = false;
            ctrlArchSoglasovaniya.Operations.DeleteInRow = false;

            if (IsObjectCreated)
            {
                wsa.DisableOperationsForNewObject(ctrlArchSoglasovaniya);
            }
        }

        /// <summary>
        /// Получить видимость кнопки подписание в подалке.
        /// </summary>
        /// <param name="pk"></param>
        /// <returns></returns>
        [WebMethod]
        public static bool GetSignBtn(string pk)
        {
            var sogl = new СогласованиеДокумента();
            sogl.SetExistObjectPrimaryKey(pk);
            UnityGlobal.DataService.LoadObject(СогласованиеДокумента.Views.СогласованиеДокументаWebE, sogl);

            return sogl.АрхивСогласования.Cast<АрхивСогласования>().OrderByDescending(x => x.CreateTime).FirstOrDefault(x =>
                x.Подписант && !x.Подписано && Tools.Constants.EQPK(x.Исполнитель?.__PrimaryKey, Сотрудник.Текущий.__PrimaryKey)) != null;
        }

        /// <summary>
        /// Перед формированием файла, проверим наличие номенклатуры.
        /// </summary>
        /// <param name="pkDoc">Ключ документа</param>
        [WebMethod]
        public static bool CheckNomenclature(string pkDoc)
        {
            if (!string.IsNullOrEmpty(pkDoc)) {
                var doc = new Документ();
                doc.SetExistObjectPrimaryKey(pkDoc);
                UnityGlobal.DataService.LoadObject(Документ.Views.ДокументWebL, doc);

                return doc.НоменклатураРегНомера != null;
            }

            return false;
        }

        /// <summary>
        /// Подписать файл.
        /// </summary>
        protected void CtrlПодписать_Click(object sender, EventArgs e)
        {
            var listDataObjects = new List<DataObject>();
            var valuePostback = Request["__EVENTARGUMENT"];

            if (!string.IsNullOrEmpty(valuePostback))
            {
                var sigFile = valuePostback.Split(',')[0];
                var modal = bool.Parse(valuePostback.Split(',')[1]);

                // Вызов метода для сохранения подписи
                SoglasovanieHelper.SaveSignature(sigFile, DataObject, listDataObjects, DataService);

                if (modal)
                {
                    var последнийАрхивСогласования = DataObject.АрхивСогласования.Cast<АрхивСогласования>().OrderByDescending(x => x.CreateTime).FirstOrDefault(x =>
                            x.Подписант && (modal || x.Согласовано == Согласовано.Да) && !x.Подписано && Tools.Constants.EQPK(x.Исполнитель?.__PrimaryKey, Сотрудник.Текущий.__PrimaryKey));

                    последнийАрхивСогласования.Подписано = true;

                    DataService.UpdateObject(последнийАрхивСогласования);
                    ctrlСогласоватьВернутьИсполнителю_Click(sender, e);
                }
                else
                {

                    listDataObjects.Clear();

                    var последнийАрхивСогласования = DataObject.АрхивСогласования.Cast<АрхивСогласования>().OrderByDescending(x => x.CreateTime).FirstOrDefault(x =>
                        x.Подписант && (modal || x.Согласовано == Согласовано.Да) && !x.Подписано && Tools.Constants.EQPK(x.Исполнитель?.__PrimaryKey, Сотрудник.Текущий.__PrimaryKey));

                    последнийАрхивСогласования.Подписано = true;
                    listDataObjects.Add(последнийАрхивСогласования);

                    // Вызов нового метода для обновления архива и добавления следующего подписанта
                    SoglasovanieHelper.UpdateArchAndAddNextPodpisant(DataObject, Сотрудник.Текущий, последнийАрхивСогласования, listDataObjects, DataService,
                        ctrlМодалСогФайлПроектаUpload, ctrlМодалСогФайлСхемаЗУUpload, ctrlМодалСогФайлДопUpload, commentSog.Value);

                    Response.Redirect(Request.RawUrl);
                }
            }
        }

        protected void ctrlОтправитьНаСогласованиеПодписание_Click(object sender, EventArgs e)
        {
            try
            {
                if (!Page_IsValid())
                {
                    return;
                }

                base.SaveObject();

                var list = new List<DataObject>();

                SoglasovanieHelper.FileHandler(DataObject, ctrlМодалОтпрСогФайлПроектаUpload, ctrlМодалОтпрСогФайлСхемаЗУUpload, ctrlМодалОтпрСогФайлДопUpload);

                var archOsnIsp = DataObject.АрхивСогласования.Cast<АрхивСогласования>().Where(x => x.Исполнитель.__PrimaryKey.Equals(DataObject.ОсновнойИсполнитель.__PrimaryKey))
                    .OrderByDescending(x => x.ДатаНазначения).FirstOrDefault();

                archOsnIsp.ДатаИсполения = DateTime.Now;
                archOsnIsp.ФайлПроекта = DataObject.ПоследнийЗагруженныйФайл;
                archOsnIsp.ФайлСоСхемойЗУ = DataObject.ФайлСоСхемойЗУ;
                archOsnIsp.ДопФайл = DataObject.ДополнительныйФайл;
                archOsnIsp.КомментарийИсполнителя = commentOtpSog.Value;

                list.Add(archOsnIsp);

                if (DataObject.Согласующий.Count > 0)
                {
                    var soglas = DataObject.Согласующий.Cast<Согласующий>().ToList().OrderBy(x => x.ПорядковыйНомерСогласующего).FirstOrDefault();
                    DataObject.ТекущийИсполнитель = soglas.СогласующийСотрудник;

                    var archSog = new АрхивСогласования()
                    {
                        Согласование = DataObject,
                        Исполнитель = soglas.СогласующийСотрудник,
                        ДатаНазначения = DateTime.Now,
                        ОсновнойИсполнитель = false,
                        Согласующий = true,
                        Подписант = false,
                        Согласовано = Согласовано.Пусто
                    };

                    DataObject.АрхивСогласования.Add(archSog);
                    DataObject.Состояние = СостояниеСогласования.ВПроцессе;
                    DataObject.ДатаОтправкиНаСогласование = DateTime.Now;
                }
                else
                {
                    var podpisant = DataObject.Подписант.Cast<Подписант>().ToList().OrderBy(x => x.ПорядковыйНомерПодписанта).FirstOrDefault();
                    DataObject.ТекущийИсполнитель = podpisant.ПодписантСотрудник;

                    var archPod = new АрхивСогласования()
                    {
                        Согласование = DataObject,
                        Исполнитель = podpisant.ПодписантСотрудник,
                        ДатаНазначения = DateTime.Now,
                        ОсновнойИсполнитель = false,
                        Согласующий = false,
                        Подписант = true,
                        Согласовано = Согласовано.Пусто
                    };

                    DataObject.АрхивСогласования.Add(archPod);
                    DataObject.Состояние = СостояниеСогласования.НаПодписании;
                }

                DataService.LoadObject(Документ.Views.ДокументWebE, DataObject.Документ);
                DataObject.Документ.Состояние = СостояниеДокумента.НаСогласовании;

                DataObject.ОтправленоНаСогласование = true;

                list.Add(DataObject);
                list.Add(DataObject.Документ);

                var arrayToSave = list.ToArray();

                DataService.UpdateObjects(ref arrayToSave);

                if (isIframe)
                    PageContentManager.AttachJavaScriptCode($"window.parent.location.reload();", true);
                else
                    Response.Redirect(Request.RawUrl);
            }
            catch(Exception ex)
            {
                LogService.LogError(ex);
            }
        }

        protected void ctrlСогласоватьВернутьИсполнителю_Click(object sender, EventArgs e)
        {
            if (!Page_IsValid())
            {
                return;
            }

            base.SaveObject();

            var list = new List<DataObject>();

            SoglasovanieHelper.AgreeReturnToContractor(DataObject, DataService, ctrlSoglasovano.SelectedValue, commentSog.Value, list,
                ctrlМодалСогФайлПроектаUpload, ctrlМодалСогФайлСхемаЗУUpload, ctrlМодалСогФайлДопUpload);  

            if (isIframe)
                PageContentManager.AttachJavaScriptCode($"window.parent.location.reload();", true);
            else
                Response.Redirect(Request.RawUrl);
        }
```

**Описание**

