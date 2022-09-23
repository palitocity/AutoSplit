# AutoSplit



#listpart
page 98734 "Payment Subform"
{
    Caption = 'Payment Subform';
    PageType = ListPart;
    ApplicationArea = All;
    UsageCategory = Lists;
    SourceTable = "Payment Line";
    DelayedInsert = true;
    LinksAllowed = false;
    MultipleNewLines = true;
    AutoSplitKey = true;
    //ShowMandatory = Type <> Type::" ";
    layout
    {
        area(Content)
        {
            repeater(GroupName)
            {

                field(ID; Rec.ID)
                {
                    Caption = 'ID';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the ID field.';
                }
                field("Doc Type"; Rec."Doc Type")
                {
                    Caption = 'Doc Type';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the Doc Type field.';
                }
                field(No; Rec.No)
                {
                    Caption = 'No';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the No field.';
                }
                field(Description; Rec.Description)
                {
                    Caption = 'Description';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the Description field.';
                }
                field("Line No"; Rec."Line No")
                {
                    Caption = 'Line No';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the Line No field.';
                }
                field(Location; Rec.Location)
                {
                    Caption = 'Location';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the Location field.';
                }
                field(Quantity; Rec.Quantity)
                {
                    Caption = 'Quantity';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the Quantity field.';
                }
                field("Total Price"; Rec."Total Price")
                {
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the Total Price field.';
                }
                field("Unit Price"; Rec."Unit Price")
                {
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the Unit Price field.';
                }
            }
        }
    }

    actions
    {
        area(Processing)
        {
            action(ActionName)
            {
                ApplicationArea = All;

                trigger OnAction();
                begin

                end;
            }
        }
    }
}

#listpart






page 76323 "Payment Card Page"
{
    Caption = 'Payment Card Page';
    PageType = Card;
    ApplicationArea = All;
    UsageCategory = Documents;
    SourceTable = "Payment Header";

    layout
    {
        area(Content)
        {
            group(GroupName)
            {

                field("Customer Name"; Rec."Customer Name")
                {
                    Caption = 'Customer Name';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the Customer Name field.';
                }
                field("Customer No"; Rec."Customer No")
                {
                    Caption = 'Customer No';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the Customer No field.';
                }
                field("Date"; Rec."Date")
                {
                    Caption = 'Date';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the Date field.';
                }
                field("Expiring Date"; Rec."Expiring Date")
                {
                    Caption = 'Expiring Date';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the Expiring Date field.';
                }
                field("External Doc No"; Rec."External Doc No")
                {
                    Caption = 'External Doc No';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the External Doc No field.';
                }
            }
            part(PaymentLine; "Payment Subform")
            {
                ApplicationArea = Basic, Suite;
                Editable = true;
                Enabled = Rec."Customer Name" <> '';
                SubPageLink = ID = FIELD(id);
                UpdatePropagation = Both;
            }
        }
    }

    actions
    {
        area(Processing)
        {
            action(ActionName)
            {
                ApplicationArea = All;

                trigger OnAction()
                begin

                end;
            }
        }
    }

    var
        myInt: Integer;
}


table 84323 "Payment Header"
{
    DataClassification = ToBeClassified;
    DataCaptionFields = "id", "Customer Name";
    Caption = 'Profit Header';

    fields
    {
        field(1; id; Integer)
        {
            DataClassification = ToBeClassified;
            AutoIncrement = true;
        }
        field(2; "Customer No"; Code[200])
        {
            DataClassification = ToBeClassified;
            TableRelation = Customer."No.";
            trigger OnValidate()
            var
                Customer: Record Customer;
            begin
                Customer.Get("Customer No");
                Rec."Customer Name" := Customer.Name;
            end;

        }

        field(3; "Customer Name"; Code[200])
        {
            DataClassification = ToBeClassified;
            TableRelation = Customer;
            trigger OnValidate()
            var
                Customer: Record Customer;
            begin
                Customer.Get("Customer Name");
                Rec."Customer Name" := Customer."No.";
                Rec."Customer Name" := Customer.Name;
            end;

        }
        field(4; "Date"; Date)
        {
            DataClassification = ToBeClassified;

        }
        field(5; "Expiring Date"; Date)
        {
            DataClassification = ToBeClassified;

        }
        field(6; "External Doc No"; Code[20])
        {
            DataClassification = ToBeClassified;

        }
    }

    keys
    {
        key(Key1; id)
        {
            Clustered = true;
        }
    }

    var
        myInt: Integer;

    trigger OnInsert()
    begin

    end;

    trigger OnModify()
    begin

    end;

    trigger OnDelete()
    begin

    end;

    trigger OnRename()
    begin

    end;

}



table 78531 "Payment Line"
{
    DataClassification = ToBeClassified;
    Caption = 'Payment Line';

    fields
    {

        field(1; ID; Integer)
        {
            DataClassification = ToBeClassified;
            TableRelation = "Payment Header";
        }
        field(2; "Doc Type"; Option)
        {
            DataClassification = ToBeClassified;
            OptionMembers = comment,item,"G/L Account";
        }
        field(3; "Line No"; Integer)
        {
            DataClassification = ToBeClassified;
            AutoIncrement = true;
        }
        field(4; "No"; code[20])
        {
            DataClassification = ToBeClassified;
            TableRelation = if ("Doc Type" = const(comment)) "Standard Text" else
            if ("Doc Type" = const(item)) Item else
            if ("Doc Type" = const("G/L Account")) "G/L Account";
            trigger OnValidate()

            begin

            end;
        }
        field(5; "Description"; Code[200])
        {
            DataClassification = ToBeClassified;
        }
        field(6; "Location"; Code[40])
        {
            DataClassification = ToBeClassified;
            TableRelation = Location;

        }
        field(7; "Quantity"; Integer)
        {
            DataClassification = ToBeClassified;
        }
        field(8; "Unit Price"; Decimal)
        {
            DataClassification = ToBeClassified;
            AutoFormatType = 2;
            trigger OnValidate()
            begin
                "Total Price" := "Unit Price" * Quantity;
            end;
        }
        field(9; "Total Price"; Decimal)
        {
            DataClassification = ToBeClassified;
            AutoFormatType = 2;
        }

    }

    keys
    {
        key(Key1; ID, "Line No")
        {
            Clustered = true;
        }
    }

    var
        myInt: Integer;

    trigger OnInsert()
    begin

    end;

    trigger OnModify()
    begin

    end;

    trigger OnDelete()
    begin

    end;

    trigger OnRename()
    begin

    end;

}


page 89332 "Payment Card List"
{
    PageType = List;
    ApplicationArea = All;
    UsageCategory = Lists;
    SourceTable = "Payment Header";

    layout
    {
        area(Content)
        {
            repeater(GroupName)
            {

                field("Customer No"; Rec."Customer No")
                {
                    Caption = 'Customer No';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the Customer No field.';
                }
                field("Customer Name"; Rec."Customer Name")
                {
                    Caption = 'Customer Name';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the Customer Name field.';
                }
                field("Date"; Rec."Date")
                {
                    Caption = 'Date';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the Date field.';
                }
                field("Expiring Date"; Rec."Expiring Date")
                {
                    Caption = 'Expiring Date';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the Expiring Date field.';
                }
                field("External Doc No"; Rec."External Doc No")
                {
                    Caption = 'External Doc No';
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the External Doc No field.';
                }
            }
        }
        area(Factboxes)
        {

        }
    }

    actions
    {
        area(Processing)
        {
            action(ActionName)
            {
                ApplicationArea = All;

                trigger OnAction();
                begin

                end;
            }
        }
    }
}








