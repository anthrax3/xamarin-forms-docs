---
title: Overview
page_title: Columns Overview
position: 0
slug: datagrid-columns-overview
---

# Columns Overview #

You can add columns in your RadDataGrid by working with the Columns collection of the control. Generally, there are three approaches that you can take to define differrent columns:

* **Manually**: by adding columns to the RadDataGrid.Columns collection
* **Automatically**: by setting RadDataGrid.AutoGenerateColumns="True"
* **Mixed**: by adding columns to the RadDataGrid.Columns collection and also set RadDataGrid.AutoGenerateColumns="True"

The **RadDataGrid** control provides the following type of columns:

* **Template Column**: Represents a column that uses a DataTemplate to describe the content of each associated grid cell.
* **Boolean Column**: A special DataGridTypedColumn implementation that presents boolean data.
* **Date Column:** An extended DataGridTextColumn that presents data of type DateTime. 
* **Numerical Column**: Represents an extended DataGridTextColumn that presents numerical data(int and double types). 
* **Text Column**: Represents a column that converts the content of each associated cell to a System.String object.
* **Time Column**: Represents an extended DataGridTextColumn that presents the **TimeOfDay** of a **DateTime** type. 
* **Picker Column**: Represents an extended DataGridTextColumn that uses a Picker editor to select value from a collection. 


>note When RadDataGrid.AutoGenerateColumns="True" the RadDataGrid generates typed columns depending on the underlying data type.

## Properties

All types of columns inherit from the **DataGridColumn** class which provides the following properties:

* **CellDecorationStyle**: Gets or sets the Style object that defines the background of each cell associated with this column. The TargetType property of the Style object is Rectangle.
* **CellDecorationStyleSelector**: Gets or sets the StyleSelector instance that allows for dynamic decoration on a per cell basis.
Header - Gets or sets the content to be displayed in the Header UI that represents the column.
* **HeaderStyle**: Gets or sets the Style instance that defines the appearance of the DataGridColumnHeader control.
* **SizeMode**: Gets or sets the DataGridColumnSizeMode value that controls how the column and its associated cells are sized horizontally.
  * Fixed: The column has a fixed width as defined by its Width property.
  * Stretch: The column is stretched to the available width proportionally to its desired width.
  * Auto: The columns is sized to its desired width. That is the maximum desired width of all associated cells.
* **Width**: - Gets or sets the fixed width for the column. Applicable when the SizeMode property is set to DataGridColumnSizeMode.Fixed.
* **ActualWidth**: Gets the actual width of the column.
* **IsAutoGenerated**: Gets a value indication whether the column is auto-generated internally.
* **CanUserEdit** (bool): Gets or sets a value indicating whether the user can edit the values in this column.
* **IsVisible** (bool): Gets a value indicating if a specific column should be visualized.

## Example - Adding columns to the RadDataGrid

	  <Grid>
        <telerikGrid:RadDataGrid AutoGenerateColumns="False" 
                                 ItemsSource="{Binding Clubs}"
                                 UserEditMode="Cell"
                                 x:Name="grid">
            <telerikGrid:RadDataGrid.Columns>

                <telerikGrid:DataGridTextColumn PropertyName="Name"
                                                HeaderText="Name">
                    <telerikGrid:DataGridTextColumn.CellContentStyle>
                        <telerikGrid:DataGridTextCellStyle TextColor="Green" 
                                                           FontSize="15" 
                                                           SelectedTextColor="Orange"  />
                    </telerikGrid:DataGridTextColumn.CellContentStyle>
                </telerikGrid:DataGridTextColumn>

                <telerikGrid:DataGridNumericalColumn PropertyName="StadiumCapacity" 
                                                     HeaderText="Stadium Capacity"/>

                <telerikGrid:DataGridBooleanColumn PropertyName="IsChampion" 
                                                   HeaderText="Champion?"/>

                <telerikGrid:DataGridDateColumn PropertyName="Established"
                                                HeaderText="Date Established"/>

                <telerikGrid:DataGridPickerColumn PropertyName="Country"
                                                  HeaderText="Country"
                                                  ItemsSourcePath="Countries"/>

                <telerikGrid:DataGridTemplateColumn HeaderText="Template Column">
                    <telerikGrid:DataGridTemplateColumn.CellContentTemplate>
                        <DataTemplate>
                            <StackLayout>
                                <Grid BackgroundColor="Orange"
                                      Margin="0, 10, 0, 0">
                                    <Label Text="{Binding Country}" 
                                           Margin="0, 5, 0, 5"
                                           HorizontalOptions="Center"
                                           VerticalTextAlignment="Center"/>
                                </Grid>
                                <Label Text="Some Custom Text" 
                                       TextColor="DarkGreen"
                                       FontSize="10"></Label>
                            </StackLayout>
                            
                        </DataTemplate>
                    </telerikGrid:DataGridTemplateColumn.CellContentTemplate>
                </telerikGrid:DataGridTemplateColumn>

                <telerikGrid:DataGridTimeColumn PropertyName="Established" 
                                                HeaderText="Time Column"/>
            </telerikGrid:RadDataGrid.Columns>
        </telerikGrid:RadDataGrid>
    </Grid>

Where the **telerikGrid** namespace is the following:

	 xmlns:telerikGrid="clr-namespace:Telerik.XamarinForms.DataGrid;assembly=Telerik.XamarinForms.DataGrid"

The **ViewModel** class is declared as following:

	public class ViewModel
    {
        private ObservableCollection<Club> clubs;

        public ObservableCollection<Club> Clubs
        {
            get
            {
                if (this.clubs == null)
                {
                    this.clubs = this.CreateClubs();
                }

                return this.clubs;
            }
        }

        private ObservableCollection<Club> CreateClubs()
        {
            ObservableCollection<Club> clubs = new ObservableCollection<Club>();
            Club club;

            club = new Club("Liverpool", new DateTime(1892, 1, 1), 45362, "England");
            clubs.Add(club);

            club = new Club("Manchester Utd.", new DateTime(1878, 1, 1), 76212, "England");
            club.IsChampion = true;
            clubs.Add(club);

            club = new Club("Chelsea", new DateTime(1905, 1, 1), 42055, "England");
            clubs.Add(club);

            club = new Club("Barcelona", new DateTime(1899, 1, 1), 99354, "Spain");
            clubs.Add(club);

            return clubs;
        }
    }
	
And the **Club** custom object:

	 public class Club : INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;

        private string name;
        private DateTime established;
        private int stadiumCapacity;
        private bool isChampion;
        private string country;
       
        public List<string> Countries
        {
            get
            {
                return new List<string>() { "England", "Spain", "France", "Bulgaria" };
            }
        }

        public string Country
        {
            get { return country; }
            set
            {
                country = value;
                this.OnPropertyChanged("Country");
            }
        }

        public bool IsChampion
        {
            get { return this.isChampion; }
            set
            {
                isChampion = value;
                this.OnPropertyChanged("IsChampion");
            }
        }

        public string Name
        {
            get { return this.name; }
            set
            {
                if (value != this.name)
                {
                    this.name = value;
                    this.OnPropertyChanged("Name");
                }
            }
        }

        public DateTime Established
        {
            get { return this.established; }
            set
            {
                if (value != this.established)
                {
                    this.established = value;
                    this.OnPropertyChanged("Established");
                }
            }
        }

        public int StadiumCapacity
        {
            get { return this.stadiumCapacity; }
            set
            {
                if (value != this.stadiumCapacity)
                {
                    this.stadiumCapacity = value;
                    this.OnPropertyChanged("StadiumCapacity");
                }
            }
        }

        public Club(string name, DateTime established, int stadiumCapacity, string country)
        {
            this.Name = name;
            this.Established = established;
            this.StadiumCapacity = stadiumCapacity;
            this.Country = country;
        }

        protected virtual void OnPropertyChanged(PropertyChangedEventArgs args)
        {
            PropertyChangedEventHandler handler = this.PropertyChanged;
            if (handler != null)
            {
                handler(this, args);
            }
        }

        private void OnPropertyChanged(string propertyName)
        {
            this.OnPropertyChanged(new PropertyChangedEventArgs(propertyName));
        }
    }


