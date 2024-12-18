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
using ICSSoft.STORMNET;
using ICSSoft.STORMNET.Business;
using IIS.ISUZ.Controls;
using IIS.ISUZ.Tools;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;

namespace IIS.ISUZ.Helpers
{
    public static class SoglasovanieHelper
    {
        /// <summary>
        /// Метод для сохранения подписи.
        /// </summary>
        public static void SaveSignature(string sigFile, СогласованиеДокумента dataObject, List<DataObject> listDataObjects, IDataService dataService)
        {
            var файлПодписи = new Файл { Имя = $"{dataObject.ПоследнийЗагруженныйФайл.Имя}.sig" };
            byte[] signature = Convert.FromBase64String(sigFile);
            файлПодписи.СохранитьФайл(signature, nameof(СогласованиеДокумента));

            dataObject.ФайлПодписи = файлПодписи;
            listDataObjects.Add(dataObject);

            // Обновление объектов
            UpdateObjects(listDataObjects, dataService);
        }

        /// <summary>
        /// Согласовать/вернуть исполнителю.
        /// </summary>
        public static void AgreeReturnToContractor(СогласованиеДокумента dataObject, IDataService dataService, string isSoglasovano, string commentSog, List<DataObject> listDataObjects,
            FileUpload ctrlМодалСогФайлПроектаUpload, FileUpload ctrlМодалСогФайлДопUpload, FileUpload ctrlМодалСогФайлСхемаЗУUpload)
        {           
            if (isSoglasovano == "Да")
            {
                var archTekucshiiIdpolnitel = dataObject.АрхивСогласования.Cast<АрхивСогласования>().Where(x => Constants.EQPK(x.Исполнитель?.__PrimaryKey, Сотрудник.Текущий.__PrimaryKey))
                    .OrderByDescending(x => x.ДатаНазначения).FirstOrDefault();

                if (archTekucshiiIdpolnitel != null)
                {
                    SetFieldArch(archTekucshiiIdpolnitel, Согласовано.Да, ctrlМодалСогФайлПроектаUpload, ctrlМодалСогФайлСхемаЗУUpload, ctrlМодалСогФайлДопUpload, commentSog);

                    dataObject.СогласованоПредпИсполнителем = Согласовано.Да;
                    dataObject.ОтправленоНаСогласование = true;

                    listDataObjects.Add(archTekucshiiIdpolnitel);
                }
                else
                {
                    LogService.LogError("Не удалось найти архив согласования для текущего исполнителя " + Сотрудник.Текущий.__PrimaryKey);
                    return;
                }

                var tekSoglas = dataObject.Согласующий.Cast<Согласующий>().Where(x => Constants.EQPK(x.СогласующийСотрудник?.__PrimaryKey, Сотрудник.Текущий.__PrimaryKey)).FirstOrDefault();

                if (tekSoglas != null)
                {
                    AddNextSoglas(dataObject, dataService, tekSoglas, listDataObjects);
                }
                else
                {
                    AddNextPodpisant(archTekucshiiIdpolnitel, listDataObjects, dataObject, Сотрудник.Текущий, dataService);
                }
            }
            else
            {
                FileHandler(dataObject, ctrlМодалСогФайлПроектаUpload, ctrlМодалСогФайлСхемаЗУUpload, ctrlМодалСогФайлДопUpload);

                var archTekucshiiIdpolnitel = dataObject.АрхивСогласования.Cast<АрхивСогласования>().Where(x => Constants.EQPK(x.Исполнитель?.__PrimaryKey, Сотрудник.Текущий.__PrimaryKey))
                    .OrderByDescending(x => x.ДатаНазначения).FirstOrDefault();

                if (archTekucshiiIdpolnitel != null)
                {
                    SetFieldArch(archTekucshiiIdpolnitel, Согласовано.Нет, ctrlМодалСогФайлПроектаUpload, ctrlМодалСогФайлСхемаЗУUpload, ctrlМодалСогФайлДопUpload, commentSog);
                    dataObject.СогласованоПредпИсполнителем = Согласовано.Нет;
                    dataObject.ОтправленоНаСогласование = false;

                    listDataObjects.Add(archTekucshiiIdpolnitel);
                }
                else
                {
                    LogService.LogError("Не удалось найти архив согласования для текущего исполнителя " + Сотрудник.Текущий.__PrimaryKey);
                    return;
                }

                var archOsnIsp = new АрхивСогласования
                {
                    Согласование = dataObject,
                    Исполнитель = dataObject.ОсновнойИсполнитель,
                    ДатаНазначения = DateTime.Now,
                    ОсновнойИсполнитель = true,
                    Согласующий = false,
                    Подписант = false,
                    Согласовано = Согласовано.Пусто
                };

                dataObject.АрхивСогласования.Add(archOsnIsp);
                dataObject.ТекущийИсполнитель = dataObject.ОсновнойИсполнитель;
                dataObject.Состояние = СостояниеСогласования.НаДоработке;
            }

            if (!listDataObjects.Any(x => Constants.EQPK(x.__PrimaryKey, dataObject.__PrimaryKey)))
            {
                listDataObjects.Add(dataObject);
            }

            // Обновление объектов
            UpdateObjects(listDataObjects, dataService);
        }

        /// <summary>
        /// Добавить следующего согласующего
        /// </summary>
        /// <param name="tekSoglas">Согласующий.</param>
        /// <param name="list">Список на обновление.</param>
        private static void AddNextSoglas(СогласованиеДокумента dataObject, IDataService dataService, Согласующий tekSoglas, List<DataObject> list)
        {
            if (tekSoglas.ПорядковыйНомерСогласующего < dataObject.Согласующий.Count)
            {
                var nextSoglas = dataObject.Согласующий.Cast<Согласующий>().Where(x => x.ПорядковыйНомерСогласующего == (tekSoglas.ПорядковыйНомерСогласующего + 1)).FirstOrDefault();
                if (nextSoglas != null)
                {
                    dataObject.ТекущийИсполнитель = nextSoglas.СогласующийСотрудник;
                    var archSog = new АрхивСогласования()
                    {
                        Согласование = dataObject,
                        Исполнитель = nextSoglas.СогласующийСотрудник,
                        ДатаНазначения = DateTime.Now,
                        ОсновнойИсполнитель = false,
                        Согласующий = true,
                        Подписант = false,
                        Согласовано = Согласовано.Пусто
                    };

                    dataObject.АрхивСогласования.Add(archSog);
                }
                else
                {
                    LogService.LogError("Не удалось найти следующего согласасующего с порядковым номером " + (tekSoglas.ПорядковыйНомерСогласующего + 1));
                    return;
                }
            }
            else
            {
                if (dataObject.Подписант.Count > 0)
                {
                    var podpisant = dataObject.Подписант.Cast<Подписант>().ToList().OrderBy(x => x.ПорядковыйНомерПодписанта).FirstOrDefault();
                    dataObject.ТекущийИсполнитель = podpisant.ПодписантСотрудник;
                    dataObject.Состояние = СостояниеСогласования.НаПодписании;

                    var archPod = new АрхивСогласования()
                    {
                        Согласование = dataObject,
                        Исполнитель = podpisant.ПодписантСотрудник,
                        ДатаНазначения = DateTime.Now,
                        ОсновнойИсполнитель = false,
                        Согласующий = false,
                        Подписант = true,
                        Согласовано = Согласовано.Пусто
                    };

                    dataObject.АрхивСогласования.Add(archPod);
                }
                else
                {
                    dataObject.Состояние = СостояниеСогласования.НаРегистрации;

                    dataObject.ТекущийИсполнитель = dataObject.Регистратор;
                    var archReg = new АрхивСогласования()
                    {
                        Согласование = dataObject,
                        Исполнитель = dataObject.Регистратор,
                        ДатаНазначения = DateTime.Now,
                        ОсновнойИсполнитель = false,
                        Согласующий = false,
                        Подписант = false,
                        Согласовано = Согласовано.Пусто
                    };
                    dataObject.АрхивСогласования.Add(archReg);
                    dataService.LoadObject(Документ.Views.ДокументWebE, dataObject.Документ);
                    dataObject.Документ.Состояние = СостояниеДокумента.НаРегистрации;

                    list.Add(dataObject.Документ);
                }
            }
        }

        /// <summary>
        /// Метод для обновления архива и добавления следующего подписанта.
        /// </summary>
        public static void UpdateArchAndAddNextPodpisant(СогласованиеДокумента dataObject, Сотрудник текущийСотрудник, АрхивСогласования последнийАрхивСогласования, List<DataObject> listDataObjects, IDataService dataService, 
            FileUpload ctrlМодалСогФайлПроектаUpload, FileUpload ctrlМодалСогФайлСхемаЗУUpload, FileUpload ctrlМодалСогФайлДопUpload, string commentSog)
        {
            // Обновление полей архива
            SetFieldArch(последнийАрхивСогласования, Согласовано.Да, ctrlМодалСогФайлПроектаUpload, ctrlМодалСогФайлСхемаЗУUpload, ctrlМодалСогФайлДопUpload, commentSog);
            listDataObjects.Add(последнийАрхивСогласования);

            // Добавление следующего подписанта
            AddNextPodpisant(последнийАрхивСогласования, listDataObjects, dataObject, текущийСотрудник, dataService);

            // Обновление объектов
            var arrayToUpdate = listDataObjects.ToArray();
            dataService.UpdateObjects(ref arrayToUpdate);
        }

        /// <summary>
        /// Метод для обновления объектов.
        /// </summary>
        private static void UpdateObjects(List<DataObject> listDataObjects, IDataService dataService)
        {
            var arrayToUpdate = listDataObjects.ToArray();
            dataService.UpdateObjects(ref arrayToUpdate);
        }

        /// <summary>
        /// Изменение полей в архиве согласования.
        /// </summary>
        public static void SetFieldArch(АрхивСогласования архив, Согласовано согласовано, FileUpload ctrlМодалСогФайлПроектаUpload, 
            FileUpload ctrlМодалСогФайлСхемаЗУUpload, FileUpload ctrlМодалСогФайлДопUpload, string commentSog)
        {
            архив.ДатаИсполения = DateTime.Now;

            var files = ctrlМодалСогФайлПроектаUpload.SaveFile();

            if (files[0] != null)
            {
                ConvertFile(ref files[0]);

                архив.ФайлПроекта = files[0];
            }

            files = ctrlМодалСогФайлСхемаЗУUpload.SaveFile();

            if (files[0] != null)
            {
                ConvertFile(ref files[0]);

                архив.ФайлСоСхемойЗУ = files[0]; // Записываем файл (может быть null)
            }

            files = ctrlМодалСогФайлДопUpload.SaveFile();

            if (files[0] != null)
            {
                архив.ДопФайл = files[0];
            }

            архив.Согласовано = согласовано;
            архив.КомментарийИсполнителя = commentSog;
        }

        /// <summary>
        /// Добавить следующего подписанта.
        /// </summary>
        public static void AddNextPodpisant(АрхивСогласования архив, List<DataObject> list, СогласованиеДокумента dataObject, Сотрудник текущийСотрудник, IDataService dataService)
        {
            var tekPodpisant = dataObject.Подписант.Cast<Подписант>()
                .FirstOrDefault(x => x.ПодписантСотрудник.__PrimaryKey.Equals(текущийСотрудник.__PrimaryKey));

            if (tekPodpisant == null)
            {
                LogService.LogError("Не удалось найти согласасующего/подписанта с ключем " + (текущийСотрудник.__PrimaryKey));
                return;
            }
            else
            {
                if (архив.Подписано)
                {
                    if (tekPodpisant.ПорядковыйНомерПодписанта < dataObject.Подписант.Count)
                    {
                        var nextPodpisant = dataObject.Подписант.Cast<Подписант>()
                            .FirstOrDefault(x => x.ПорядковыйНомерПодписанта == (tekPodpisant.ПорядковыйНомерПодписанта + 1));

                        if (nextPodpisant != null)
                        {
                            dataObject.ТекущийИсполнитель = nextPodpisant.ПодписантСотрудник;

                            var archPod = new АрхивСогласования()
                            {
                                Согласование = dataObject,
                                Исполнитель = nextPodpisant.ПодписантСотрудник,
                                ДатаНазначения = DateTime.Now,
                                ОсновнойИсполнитель = false,
                                Согласующий = false,
                                Подписант = true,
                                Согласовано = Согласовано.Пусто
                            };

                            dataObject.АрхивСогласования.Add(archPod);
                        }
                        else
                        {
                            LogService.LogError("Не удалось найти следующего подписанта с порядковым номером " + (tekPodpisant.ПорядковыйНомерПодписанта + 1));
                            return;
                        }
                    }
                    else
                    {
                        dataObject.Состояние = СостояниеСогласования.НаРегистрации;

                        dataObject.ТекущийИсполнитель = dataObject.Регистратор;
                        var archReg = new АрхивСогласования()
                        {
                            Согласование = dataObject,
                            Исполнитель = dataObject.Регистратор,
                            ДатаНазначения = DateTime.Now,
                            ОсновнойИсполнитель = false,
                            Согласующий = false,
                            Подписант = false,
                            Согласовано = Согласовано.Пусто
                        };
                        dataObject.АрхивСогласования.Add(archReg);
                        dataService.LoadObject(Документ.Views.ДокументWebE, dataObject.Документ);
                        dataObject.Документ.Состояние = СостояниеДокумента.НаРегистрации;

                        list.Add(dataObject.Документ);
                    }
                }

                list.Add(dataObject);
            }
        }

        /// <summary>
        /// Преобразование файла.
        /// </summary>
        public static void ConvertFile(ref Файл file)
        {
            var extention = Path.GetExtension(Path.Combine(file.Путь, file.Имя));
            if (extention == ".doc" || extention == ".docx")
            {
                var result = new DocToPdfConverter().ConvertDocToPdfAndDeleteSource(Path.Combine(file.Путь, file.Имя));

                if (result != null && result.TryGetValue("Path", out string outPath) && result.TryGetValue("Name", out string outName))
                {
                    file.Путь = outPath;
                    file.Имя = outName;
                }
            }
        }

        /// <summary>
        /// Обработка файловых контроллов на формах согласования и лк.
        /// </summary>
        /// <param name="согласованиеДокумента">СогласованиеДокумента.</param>
        /// <param name="ФайлПроектаUpload">Файловый контролл.</param>
        /// <param name="ФайлСхемаЗУUpload">Файловый контролл.</param>
        /// <param name="ФайлДопUpload">Файловый контролл.</param>
        public static void FileHandler(СогласованиеДокумента согласованиеДокумента, FileUpload ФайлПроектаUpload, FileUpload ФайлСхемаЗУUpload, FileUpload ФайлДопUpload)
        {
            Файл[] filesPDF = null;
            var files = ФайлПроектаUpload.SaveFile();

            if (files != null && files[0] != null)
            {
                // Делаем копию файла, чтобы основной остался doc, а скопированный стал pdf   
                var newFilePDF = new Файл();
                files[0].CopyTo(newFilePDF, false, false, false);
                filesPDF = new Файл[] { newFilePDF, null };

                ConvertFile(ref filesPDF[0]);

                согласованиеДокумента.ПоследнийЗагруженныйФайл = files[0];
            }

            files = ФайлСхемаЗУUpload.SaveFile();

            if (files != null && files[0] != null)
            {
                ConvertFile(ref files[0]);

                согласованиеДокумента.ФайлСоСхемойЗУ = files[0]; // Записываем файл (может быть null)
            }

            if (filesPDF != null && filesPDF[0] != null)
            {
                Файл файлДляШтампов = null;
                if (согласованиеДокумента.ПодготовкаСхемыЗУ && согласованиеДокумента.ФайлСоСхемойЗУ != null)
                {
                    var result = StampHelper.CombineTwoPDF(Path.Combine(filesPDF[0].Путь, filesPDF[0].Имя),
                        Path.Combine(согласованиеДокумента.ФайлСоСхемойЗУ.Путь, согласованиеДокумента.ФайлСоСхемойЗУ.Имя));

                    if (result != null && result.TryGetValue("Path", out string outPath) && result.TryGetValue("Name", out string outName))
                    {
                        файлДляШтампов = new Файл()
                        {
                            Имя = outName,
                            Путь = outPath,
                        };
                    }
                }

                согласованиеДокумента.ФайлДляШтампов = файлДляШтампов ?? new Файл()
                {
                    Имя = filesPDF[0].Имя,
                    Путь = filesPDF[0].Путь,
                };
            }

            files = ФайлДопUpload.SaveFile();

            if (files != null && files[0] != null)
            {
                согласованиеДокумента.ДополнительныйФайл = files[0];
            }
        }
    }
}

```

**Используемые принципы**

**KISS**

Принцип KISS предполагает, что код должен быть простым и понятным, без лишней сложности.

В коде используются простые и понятные методы для реализации функционала. 
Например, в методах, таких как SaveSignature и AgreeReturnToContractor, функциональность четко разделена на отдельные шаги. 
Так, например, методе SaveSignature выполняется только сохранение подписи и обновление данных, без внедрения лишней логики. 
Также вся логика по обработке файлов и изменениям состояния документа разделена на отдельные методы и помещена в отдельный файл SoglasovanieHelper (серверный код выше).

Например:

- Метод SaveSignature отвечает за сохранение подписи. Код внутри метода выполняет одну задачу: конвертирует строку в байты и сохраняет файл, а затем обновляет объект документа.
- Метод AgreeReturnToContractor достаточно компактный, но при этом функционально разделен на несколько ясных и отдельных шагов: проверка согласования, работа с архивами и добавление следующих согласующих/подписантов.

**YAGNI**

Принцип YAGNI призывает не реализовывать функциональность, которая не используется в данный момент, и не добавлять лишних возможностей.

В представленном коде не добавлено лишних функций, которые могут быть потенциально полезны в будущем. Например, методы, работающие с файлами, обрабатывают только те файлы, которые реально необходимы в контексте работы с согласованием. Нет лишних условий или «передуманных» фич.

Например:

- В методах, например, в FileHandler, файловые контроллы обрабатываются строго в рамках текущего контекста — для сохранения и преобразования документов. Никакие дополнительные функции или проверки, не связанные с этой задачей, не добавляются.

**DRY**

Принцип DRY предполагает, что код не должен содержать повторяющихся фрагментов. Логика, которая используется несколько раз, должна быть вынесена в отдельные методы.

В коде видно, что часто повторяющиеся действия, такие как обработка файлов или обновление архивов согласования, вынесены в отдельные методы, например, в SetFieldArch или FileHandler. Это минимизирует повторение кода и делает его более поддерживаемым.

Например:

- В методах SetFieldArch и FileHandler используются общие паттерны обработки файлов. Эти методы вызываются в разных частях кода, избегая повторения одинаковых фрагментов логики.
Например, метод FileHandler обрабатывает несколько типов файлов, но при этом общая логика сохранения и конвертации файлов выведена в отдельный метод ConvertFile, что делает код компактным и избегает дублирования.

**SOLID**

Принципы SOLID — это набор из пяти принципов объектно-ориентированного программирования, которые помогают создавать удобный для расширения и поддерживаемый код.

1. Single Responsibility Principle (SRP)
   Принцип единой ответственности утверждает, что класс должен иметь одну причину для изменения.
   Каждый метод в коде имеет четкую задачу. Например, метод SaveSignature отвечает только за сохранение подписи, а метод AgreeReturnToContractor только за логику согласования. Это позволяет легко поддерживать и расширять код, не затрагивая другие части системы.

3. Open/Closed Principle (OCP)
   Код должен быть открыт для расширения, но закрыт для модификации.
   Методология работы с файлами и согласованиями подразумевает, что можно добавлять новые форматы файлов или этапы согласования, не изменяя основной код. Например, если в будущем понадобится поддержка новых типов файлов, достаточно будет модифицировать только соответствующие методы, не изменяя общую структуру классов и логики.

4. Liskov Substitution Principle (LSP)
   Объекты подклассов должны быть заменяемыми объектами базового класса.
   Использование наследования, если оно имело бы место в более крупных классах (например, для разных типов файлов или сотрудников), должно позволять безошибочно заменять экземпляры подклассов.

5. Interface Segregation Principle (ISP)
   Интерфейсы должны быть специфичными и не перегруженными ненужными методами.
   В коде интерфейсы не перегружены лишними методами. Каждый интерфейс предоставляет только те методы, которые необходимы для работы с объектами в текущем контексте (например, методы для загрузки и сохранения файлов).

5. Dependency Inversion Principle (DIP)
   Зависимости должны быть инвертированы, т.е. классы должны зависеть от абстракций, а не от конкретных классов.
   Принцип DIP соблюдается через использование абстракций (например, IDataService), что позволяет легко подменять реализацию данных (например, использовать разные источники данных или сервисы).
